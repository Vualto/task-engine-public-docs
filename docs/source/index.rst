TASK ENGINE Documentation
===========================================

.. toctree::
   :maxdepth: 2
   :hidden:
   
   ALL DOCUMENTATION <https://docs.vualto.com>
   Task Engine <https://docs.vualto.com/projects/task-engine/en/latest/index.html>

The Task Engine is a product that facilitates the creation of VOD from online and offline sources. It is designed to allow simple or complicated custom workflows to be developed, deployed and maintained easily and quickly. The Task Engine is exposed via a REST API and reports back to the client system via a callback mechanism.

How it works
-----------------

The Task Engine breaks a workflow up in to several component parts. Every piece of work submitted is a job, and each job is broken up in to a series of tasks. These tasks are then scheduled on to queues which workers then process.

Every job and task have unique identities, and these are exposed back to the client system both through the response to the initial job submission and through the callbacks (unless for some reason the configuration prevents this from happening).

Callbacks are sent to the specified endpoints when each task starts and ends (successful or fail). This allows the integrated systems to keep track of the job's progress. A final callback is also submitted when a job completes. The final callback will always contain the status of the job (success or fail) and information about the assets related to the job.

Contents:
---------

.. toctree::
   :maxdepth: 2

   DeveloperDocumentation.rst
   ReleaseNotes.md
   Support.md
