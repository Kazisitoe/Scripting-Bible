<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>

# Game Development

## 4.0 - Roblox Game Development
A lot of these concepts are found in/can apply in other engines and languages, the main focus for this and from now on will be Roblox Studio. We gonna be looking at how the Roblox engine works so we can finally script in it!

<hr>

### 4.1 - Data Models
Roblox handles everything as instances of objects; models, scripts, players, they are all instances! Wow! Roblox uses a **parent**-**child** hierarchy:
<br>![Parent child example](.\assets\ParentChild.png "Parent child example")<br>
For example, here ``Workspace`` is the parent of ``Baseplate``, and ``Baseplate`` is a child of ``Workspace``. Every instance has a ``Parent`` property that says its parent and can be used to reparent. Children can be accessed via indexing the parent with the name of the child, like a property (e.g. ``Workspace.Baseplate`` or ``Workspace["Baseplate"]``) or with the ``:FindFirstChild`` method every instance has (e.g. ``Workspace:FindFirstChild("Baseplate")``). It is important to note that if a child with the name you are looking for does not exist, trying to access it via indexing will result in an error, but using ``:FindFirstChild`` will simply return ``nil`` (e.g. ``Workspace.Kazi`` would error as there is no ``Kazi`` in ``Workspace``, but ``Workspace:FindFirstChild("Kazi")`` would just return ``nil`` without erroring).

Children of children are referred to as **descendants**, and parents of parents are referred to as **ancestors**. There are appropriately ``:FindFirstAncestor`` and ``:FindFirstDescendant`` methods that work similarly to ``:FindFirstChild``. ``:FindFirstChild`` also takes a second parameter for recursion, which effectively makes it work like ``:FindFirstDescendant`` if the second argument is true. The highest ancestor is ``game``, which is of the ``DataModel`` class. Roblox has a default variable for ``game``, meaning that you could do ``game.Workspace.Baseplate``, and also has a default variable for ``workspace`` that is equal to ``game.Workspace``.

Classes for Roblox instances typically have multiple levels of inheritence. While it can be hard to keep track of, Roblox has mostly [well-made documentation](https://create.roblox.com/docs) for every class. The documentation also includes tutorials, which can be helpful, but I'm kind of skeptical of tutorials in general. Instances have the ``ClassName`` property which is always set to the specific class the instance is. They also have the ``:IsA(class)`` function which returns ``true`` if the instance's class either is the specified class or a **subclass** (class that inherits from another class) of the specified class.

<hr>

### 4.2 - Services
**Singletons** are a type of class that can only have one instance. Does that seem pointless? Well yes, that's because it is! Roblox has **services** which are singletons that are all children of ``game`` and can be accessed with ``game:GetService(serviceName)``. ``Workspace`` is a service, so it can be accessed with ``game:GetService("Workspace")``. Most services have specific methods and **events** (will get into in a later chapter) that allow you to interact with Roblox's engine in different way or perform specific tasks.

<hr>

### 4.3 - Netcode and Networking
**Netcode** describes how an application handles multiple **clients** (users) that interact with each other. The process of one client updating other clients on what it does, and vice versa, is called **replication** (e.g. whenever a player moves, their position is replicated to other clients).

The simplest method for connecting clients to each other is **peer-to-peer** (p2p). With p2p, clients just send information to every other connected client whenever they want to replicate. While this is simple, it becomes an increasingly higher and higher load on every client once more clients are connected, and if there is every a desync, it is essentially impossible to figure out which client's version of what's going on is the true one. Because of this, p2p is often only used for applications with a low amount of users that connect to each other (e.g. a game based around 1v1ing another player would work fine with p2p). The disadvantages of p2p are solved by the **client-server** architecture. With this, clients connect to a central **server** that handles all replication. Clients request to replicate information to the server, which can then either accept the request and propogate the replication to other clients, or deny it and undo it on the client that requested it. With this, it's a lot easier to conenct multiple clients, keep them synchronized, and keep track fo and validate what's happening. Roblox uses the **client-server** architecture.

Synchronizing clients itself can be handled different ways. **Delay-based** netcode is the simplest way: whenever a client is desynced, simply pause every other client until they're back in sync. If this sounds bad, that's because it is! It essentially means if one player is lagging, then every other client has to wait for them to stop lagging, and whenever the client wants to do anything, it has to wait for the server in order to see it happen. A far better method is **rollback-based** netcode. With it, all requests are simulated immedeately on the client at the same time of replication. If the server denies the replication, it does a **rollback** and undoes the simulation on the requesting client. If it accepts the replication, it propogates it to other clients. This method allows for immedeate feedback and doesn't force other players to endure another's lag. For all physics replication (e.g. movement), Roblox has built-in rollback handling. For type of replication you specifically script, you have to handle synchronization yourself if you want it, but it will never be delay-based.

<hr>

Wow! That's the overview! Now we can actually script in Roblox and learn more about how it works by example!