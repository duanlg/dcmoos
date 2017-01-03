# Data Center Manager Over OpenStack
This is a project aiming for creating an data center manager leveraging OpenStack

Data Center Manager Leveraging OpenStack
Gary Duan
EG
li-gong.duan@hp.com

Abstract
OpenStack facilates users to provision and manage cloud services in a convenient way, including compute instances, storage and network. Meanwhile, data center requires a converged, uniformed management solution to provision, monitor, manage and diagnostic servers, and even collaborate seamlessly with other existing IT solution.This data center manager addresses this requirement by providing an open-source, easy-to-customizing, converged management solution based on OpenStack technologies, plugin mechanisms.

Problem statement
There are many softwares managing machines in data center but it seems that none of them provide a solution perfectly resolving all the requirements in data center: management softwares provided by hardware vendors always focus on machines manufactured by them; the existing third-party softwares only provides high-level management due to no knowledges of specific hardwares.  
As a result, IT department has to use several management sofwares, which brings about disadvantages: increasing learning cost; struggling with different software intefaces; no easy second-development or integration with the existing IT systems. 

Our solution
The solution propose a new data center manager(henceforth, called DCM), which is customizable, modular and supporting plugins provided by third-party or hardware vendors and also leverages Ceilometer (telemetering), Ironic (baremetal), Glance (Image), Heat (collaboration) of OpenStack. 
Customizable
First, this DCM provides RESTful API, besides traditional CLI and Web UI, which facilitates easy collaboration with other IT system, such as machine lifecycle management. And even user can develop mobile app to manage data center. Second, user can define his monitoring target, event trigger threshold and select corrective actions for this event if necessary in a template. Last but not least important, IT department, hardware vendor or third-party can easily add their specific solution based on their specific requirement or their specific hardware capabilities.
Modular
As OpenStack always does, the communication between components in a project is via AMQP or OSLO messages and that between different project via RESTful API. It will provide good isolation so that the failure of one component doesn’t affect other components in the same project or in other projects.
Pluginable
Plugin interface facilitates hardware vendor to implement failure handling or diagnostics solution based on their own hardware. And it also allow to integrated the brilliant features of current system management software available.
This data center manager provides 3 basic functionalities:
Deploy and Configure
Currently, 2 ways to deploy and configure are in plan: golden images and Puppet plus OpenStack Heat. The former one facilitates users to deploy on a large number of the identical hardwares with their own customized golden image, which leverages Glance; the latter is the current Puppet way and it can be integrated into this data center manager through OpenStack Heat. Also, we are watching for the development of OpenStack TripleO and plan to integrate TripleO into DCM.
Monitor/Manage
All the information and status of hardware devices are displayed and once any failure and the sign of failure occurs, DCM will notify user of this event and take corrective actions (such as retiring or rebooting this machine)  or preventative actions (such as marking the bad page unavailable in DIMM if there are a number of sticky bits) based on user’s customized policy. Meanwhile, critical software or services running on these physical machines are also be monitored in the same way. User can define their monitoring target/threshold/action in the template. DCM also provides some default templates based on the role of physical machines, such as mysql database server or apache web server. These funcationalities utilize OpenStack Ceilometer and Ironic.
Diagnostics
Sometimes user is notified a hardware error event and he might want to know the possible root cause and the status of the related hardware. Diagnostics functionality serves this purpose. Online diagnostics will do this without impacting  your business or Operating system’s running and offlne diagnostics will automatically rebooting this machine and enter into UEFI and execute hardware tests which might erasing the user data.
Here is the architecture diagram.






















Evidence the solution works
OpenStack are used to provision, manage, monitor virtual machines to provide cloud services, which manifest that these technologies can work with virtual machines perfectly and as long as there is an interface which works with physical machines, these technologies should be work with physical machines. And OpenStack Ironic can work as this role. Although the original purpose of Ironic is to provision Cloud services on physical machine, it can be used as an interface to work with physical machines.
A working prototype has been implemented which address the monitoring/manage functionality, which works in our local machine room. But obviously, it need to be extended to address all the features mentioned above. 

Competitive approaches
Currently, there are 2 kinds of system management softwares: provided by hardware vendors and provided by third-party software vendors, but neither of them are considered as perfectly addressing data center’s requirement.
Softwares provided by hardware vendors might work well with their own physical hardwares but they don’t work well with, or even don’t support physical machines of other vendors and in most cases, one data centers hosts physical machines provided by different vendors. 
Softwares provided by third-party software vendors might work with all physical machines but they just provide the general or high-level features because they don’t know the details or the specifics of hardwares provided by one vendors.
The biggest advantage of this DCM is software-defined in all level. User can specify the monitor target and the threshold via template mechanism; user can also define how they are monitored and the corresponding corrective or preventive actions once an event occurs via plugin mechanism. In addition, plugin mechanism allows hardware vendors to provide their specific diagnostic solution based on their own hardware or firmware features. 

Current status
This DCM is a big picture and it requires a lot of effort to implement all the features. Currently, based on this idea, we are implementing a Cloud Device manager which are optimized for OpenStack Cloud. Once this is done, we can extend it to cover full features.

References
Ceilometer architecture:   http://docs.openstack.org/developer/ceilometer/architecture.html
Ironic architecture:  http://docs.openstack.org/developer/ironic/dev/architecture.html





