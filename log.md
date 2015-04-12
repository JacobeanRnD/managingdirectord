 Spec: https://github.com/wsargent/docker-cheat-sheet#lifecycle
  docker create creates a container but does not start it.
  docker run creates and starts a container in one operation.
  docker stop stops it.
  docker start will start it again.
  docker restart restarts a container.
  docker rm deletes a container.
  docker kill sends a SIGKILL to a container.
  docker attach will connect to a running container.
  docker wait blocks until container stops.

 Docker container lifecycle is:
  Created
  Started
    Running
      <circuit breaker logic to intercept events>
    Paused
  Stopped
    User Requested
    Crashed
      Restarting

Notes: 
This would be a perfect use case for <invoke>. 

But:
Why not let it manage its own state?
And we just listen for container events?
Use a circuit breaker to manage sending external events into the state machine. 
Persist snapshot on event, as we do. 

Then when he crashes, we restart him to the last known good state. 
This is cheap, application-level snapshotting. 
For robust network applications.
How much does it cost to save the state?

Maybe in Event Log we highlight the error states.
So this could be used to manage any system that can cheaply dump its application state.
I am not aware of anything like this that currently exists.

And visualize it. 

So then we use our dashboard as fleet control for these containers.
And that is software for a fucking orchestration platform. Boom motherfuckers.

License:
* AGPL that shit. 
Policy: basically, any SCXML code we write is application code. We AGPL for it. 
Give the platform away, charge for the applications that run on it.

So:
* We write it. Test it. 

And then we use the container manager to manage itself. 


Open questions:
  Docker:
    Not 100% clear that container events do what we think they do.

Make sure we handle every event: 
  * create, 
  * destroy, 
  * die, 
  * export, 
  * kill, 
  * pause, 
  * restart, 
  * start, 
  * stop, 
  * unpause

The decision before us is how explicit should we be in modeling these flows? Do we want to specify a top-level source state? Simple delegate to whatever docker tells us is happening? And add specific behaviour with regard to transitions from particular states?

I think we should definitely save snapshots as work with the container...
Other than that, it can be useful to visualize the state of the docker container.
And you do not really want to allow certain actions when the container is in a certain state. E.g. you do not want to call stop when the container is already stopped. 
So, basically, we keep our current states, but modify our transitions such that we transition from top-level state directly into substate for each of the above events.  
  For system.* events, we process these by sending event into docker container via dockerode script. These can be discretized depending on the state you are in. Because even though the docker container is robus to this, why allow it? 

  What becomes interesting is the saving of snapshots. To a database, or in memory. When we restart a container, we can pass snapshot in to return to last good state. This is if you have snapshot. If you do, cool, use it.  

  OK. And also, integrate circuit breaker logic. So we either queue events, send them into the container, or immediately fail them depending on what state we are in.

First prototype is simply to listen to:
* Start a docker container, 
* Listen to events on it. 
* Change states based on docker container events.
* Receive events and pass these into the state machine via dockerode. 
* Watch the system as it responds.

What image should it use? helloworld.scxml

Next prototype is to show graceful restart:
* Show an example of saving and restoring last good state. 
* Show this managing another SCXML file. 

