Bare Metal Service state report (via Guru Meditation Reports)
=============================================================

The Bare Metal service contains a mechanism whereby developers and system
administrators can generate a report about the state of running Bare Metal
executables (ironic-api and ironic-conductor). This report is called a Guru
Meditation Report (GMR for short).
GMR provides useful debugging information that can be used to obtain
an accurate view of the current live state of the system. For example,
what threads are running, what configuration parameters are in effect,
and more. The eventlet backdoor facility provides an interactive shell
interface for any eventlet-based process, allowing an administrator to
telnet to a pre-defined port and execute a variety of commands.

Configuration
-------------

The GMR feature is optional and requires the oslo.reports_ package to be
installed. For example, using pip::

    pip install 'oslo.reports>=1.18.0'

.. _oslo.reports: https://opendev.org/openstack/oslo.reports

Generating a GMR
----------------

A *GMR* can be generated by sending the *USR2* signal to any Bare Metal process
that supports it.  The *GMR* will then be output to stderr for that particular
process. For example:

Suppose that ``ironic-api`` has process ID ``6385``, and was run with
``2>/var/log/ironic/ironic-api-err.log``.  Then, sending the *USR* signal::

    kill -USR2 6385

will trigger the Guru Meditation report to be printed to
``/var/log/ironic/ironic-api-err.log``.

Structure of a GMR
------------------

The *GMR* consists of the following sections:

Package
  Shows information about the package to which this process belongs, including
  version information.

Threads
  Shows stack traces and thread IDs for each of the threads within this
  process.

Green Threads
  Shows stack traces for each of the green threads within this process (green
  threads don't have thread IDs).

Configuration
  Lists all the configuration options currently accessible via the CONF object
  for the current process.

.. only:: html

   Sample GMR Report
   -----------------

   Below is a sample GMR report generated for ``ironic-api`` service:

   .. include:: report.txt
        :literal:
