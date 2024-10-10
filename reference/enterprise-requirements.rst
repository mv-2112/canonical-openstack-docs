Enterprise Requirements
=======================

Single-node
-----------

For single-node deployments the following minimum hardware specification
applies:

+-----------------------+-----------------------+-----------------------+
| Component             | Specification         | Notes                 |
+=======================+=======================+=======================+
| CPU                   | 4 core                | amd64 only            |
+-----------------------+-----------------------+-----------------------+
| RAM                   | 16 GiB                |                       |
+-----------------------+-----------------------+-----------------------+
| Root Disk             | 100 GiB of free space | SSD                   |
+-----------------------+-----------------------+-----------------------+
| Control plane network | 1 Gbps                | Mainly localhost only |
|                       |                       | networking so minimal |
|                       |                       | requirement           |
+-----------------------+-----------------------+-----------------------+
| External network      | 1 Gbps                | Optional - only       |
|                       |                       | required for remote   |
|                       |                       | access to instances   |
+-----------------------+-----------------------+-----------------------+
| Storage               | 1 x 100 GiB SSD       | Optional - only       |
|                       |                       | required for block    |
|                       |                       | storage service       |
+-----------------------+-----------------------+-----------------------+

[note] **Note:** A single-node deployment has no resilience and has
limited performance. [/note]

Multi-node
----------

For multi-node deployments the following minimum hardware specification
applies:

+-----------------------+-----------------------+-----------------------+
| Component             | Specification         | Notes                 |
+=======================+=======================+=======================+
| CPU                   | 16 core               | amd64 only            |
+-----------------------+-----------------------+-----------------------+
| RAM                   | 32 GiB                |                       |
+-----------------------+-----------------------+-----------------------+
| Root Disk             | 500 GiB of free space | SSD                   |
+-----------------------+-----------------------+-----------------------+
| Control plane network | 1 Gbps                | Supports east/west    |
| network               |                       | traffic               |
+-----------------------+-----------------------+-----------------------+
| External network      | 1 Gbps                | Supports north/south  |
|                       |                       | traffic               |
+-----------------------+-----------------------+-----------------------+
| Storage               | 1 x 500 GiB SSD       | Required for block    |
|                       |                       | storage and image     |
|                       |                       | services              |
+-----------------------+-----------------------+-----------------------+

[note] **Note:** Three nodes are required for multi-node operation.
[/note]
