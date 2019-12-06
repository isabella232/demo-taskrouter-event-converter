# Converted Events

Here is the list of the Task Router's events and their representation as teravoz events:

## Task events
* task.created -> **call.new**
* task.canceled -> **call.queue-abandon**
* task.wrapup -> **call.finished** AND **agent.left**

## Task Queue Events
* task-queue.entered -> **call.waiting**


## Reservation Events
* reservation.accepted -> **actor.entered** AND **call.ongoing**

## Worker Events
*  worker.activity.update -> varies considering the WorkerActivityName:
* * If the WorkerActivityName is equal to `available` and he was not already available on the queue, then an **actor.logged-in** event is produced
* * If the WorkerActivityName is equal to `unavailable` or `offline`, while the agent was already not in these two status, then an **actor.logged-out** is produced
* * If the WorkerActivityName is equal to `break`, and the user was not already in the `break` status, then an **actor.paused** is produced
* * At least, if the WorkerActivityName is equal to available and the agent was in a break, then an **actor.unpaused** is produced.

# Points to Review

* When the Task enters in the wrapup state (task.wrapup event), two Teravoz's events are fired: `call.finished` AND `agent.left`. However, maybe the actor.left event should be fired when the task finishes (task.completed event), because only on the end of the wrapup status the agent will be available again. 

* When a `actor.noanswer` is produced, the `ringtime` is equals to the task age on Twilio's Task Router, not the time that was ringing to the extension that have rejected or not answered the call.

# Missing fields

## General

* **code** - This field is intended to be provided by user when triggering the dialer, to recognize an specific call in the events.

## call.new

* **their_number_type** - This info is not provided when the taskRouter callback is triggered on task.new

## actor.* events

* **queue** - The change of status using the TaskRouter is not related to a queue, so this parameter is not provided by Twilio.

