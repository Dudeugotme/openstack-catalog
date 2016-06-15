# Openstack Kolla Deployment
---

### Purpose
This will install Openstack Mitaka using Kolla on the nodes in your environment. The Openstack services will be managed via Kolla and show up as `Standalone Containers` in Rancher.

Once installed, you will have a basic Openstack deployment with the Cirros image and standard VM flavors available to you. 

A good reference for additional information can be found on the [Kolla Quickstart Guide](http://docs.openstack.org/developer/kolla/quickstart.html)

### Pre-Reqs

 * At least 2 Rancher hosts
 	* 1 host of labeled `openstack=controller` with 8GB of Ram and 40GB disk.
 	* N hosts labeled with `openstack=compute` with >=4GB Ram and 40GB disk.
 * KVM enabled hosts
 * Networking needs to be preconfigured. 
 	* You will need 1 interface for the APIs and management, and one for Neutron.
 * /run needs to be shared on the host. 
 	* `mount --make-shared /run` 
 	
 		-- or --
 	
 	* For systemd systems
 	
 	```
 	# Create the drop-in unit directory for docker.service
mkdir -p /etc/systemd/system/docker.service.d

	# Create the drop-in unit file
	tee /etc/systemd/system/docker.service.d/kolla.conf <<-'EOF'
	[Service]
	MountFlags=shared
	EOF

	# Run these commands to reload the daemon
	systemctl daemon-reload
	systemctl restart docker
	```

 * Ensure Libvirt is not running base os.

### Basics
 
 * How do I login to Horizon?
 	* The project is `default` the username created is `admin` and the password is what was set as Openstack admin password.
 	* If its not working, on the controller node in `/etc/kolla/admin-openrc.sh the OS_PASSWORD should be available.
 * How do I setup my networks?
 	* You can use the Horizon UI
 	* Use Rancher UI to get a shell into the `openstackcontroller` container. Source `/etc/kolla/admin-openrc.sh` and use the Openstack command line tools. (ie. `neutron`)