Prerequisites for Software
==========================

Hardware Requirements
---------------------

To ensure codec performance, as well as inference performance, make sure that your onboard computer has the following minimum operating requirements:

+----------------+----------------------+----------------------------------+----------------------------------+
| Components     | Minimum Requirements | Used for                         | Reasoning                        |
+================+======================+==================================+==================================+
| CPUs           | 4 Cores              | Video streaming for remote       | These operations use software    |
|                |                      | teleoperation and image          | codecs, and low CPU performance  |
|                |                      | compression of dataset creation  | can result in encoding frame     |
|                |                      |                                  | rates not keeping up with        |
|                |                      |                                  | camera frame rates               |
+----------------+----------------------+----------------------------------+----------------------------------+
| RAM            | 8 GB                 | Dataset creation cache           | Camera images are cached in      |
|                |                      |                                  | memory for a period of time      |
|                |                      |                                  | before being written to disk     |
+----------------+----------------------+----------------------------------+----------------------------------+
| GPU            | NVIDIA RTX2060       | Inference of VLA models          | AhaRobot uses NVIDIA GPUs for    |
|                |                      |                                  | inference acceleration           |
+----------------+----------------------+----------------------------------+----------------------------------+

Software Requirements
---------------------

The main part of AhaRobot runs on ROS2 Humble and employs Docker to isolate the main system. Make sure you have the following software components installed on your onboard computer:

+----------------+----------------------------------+
| Components     | Installation Instructions        |
+================+==================================+
| Ubuntu 24.04.1 | `Ubuntu Desktop`_                |
| Desktop        |                                  |
+----------------+----------------------------------+
| Docker         | `Docker Installation`_           |
+----------------+----------------------------------+
| NVIDIA         | `NVIDIA Container Toolkit`_      |
| Container      |                                  |
| Toolkit        |                                  |
+----------------+----------------------------------+
| Sunshine       | `Sunshine Documentation`_        |
| (recommended)  |                                  |
|                | Every time you reboot your       |
|                | system, you need to start the    |
|                | service in the terminal with     |
|                | this command:                    |
|                | ``systemctl --user restart       |
|                | sunshine && DISPLAY=:0 xhost     |
|                | local: +``                       |
+----------------+----------------------------------+

.. _Ubuntu Desktop: https://ubuntu.com/download/desktop
.. _Docker Installation: https://docs.docker.com/engine/install/ubuntu/
.. _NVIDIA Container Toolkit: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html
.. _Sunshine Documentation: https://docs.lizardbyte.dev/projects/sunshine/latest/md_docs_2getting__started.html
