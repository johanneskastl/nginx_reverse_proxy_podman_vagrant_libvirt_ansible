# nginx_reverse_proxy_podman_vagrant_libvirt_ansible

Vagrant-libvirt setup that installs Podman in a VM and creates two containers.

One container is running Nginx, serving a small webpage only saying `Hello from
vagrant-libvirt`.

The other Nginx container is acting as a reverse proxy and is forwarding
requests for `nginx.192.0.2.13.sslip.io` to the other server, while it serves
Nginx's default page when being called by e.g. its IP address only.

The `main` branch that you are currently on is using Podman Quadlets to manage
the containers.

The second branch `non-quadlet_containers` uses ... well, non-quadlet
containers.

Default OS is AlmaLinux 9, but that can be changed in the Vagrantfile.
Please be aware, that this might break the Ansible provisioning.

## Vagrant

1. You need `vagrant`, obviously. And `git`. And Ansible...
1. Fetch the box, per default this is `almalinux/9`, using
   `vagrant box add almalinux/9`.
1. Make sure the git submodules are fully working by issuing
   `git submodule init && git submodule update`
1. Run `vagrant up`
1. Ansible prints out three URLs at the end of the provisioning step:

   ```
   TASK [Nginx server URL without proxy] ******************************************
   ok: [nginx-podman] => {
       "msg": "URL: http://192.0.2.13:8080"
   }

   TASK [Nginx proxy URL] *********************************************************
   ok: [nginx-podman] => {
       "msg": "URL: http://192.0.2.13:80"
   }

   TASK [Nginx URL via reverse proxy] *********************************************
   ok: [nginx-podman] => {
       "msg": "URL: http://nginx.192.0.2.13.sslip.io"
   ```

1. Run `curl` against each of these URLs. You should get the `Hello from
   vagrant-libvirt` from the first one (with port 8080).
   You should get the nginx default page from the second one (with port 80).
1. Running curl against the `nginx.192.0.2.13.sslip.io` address should again
   give you the `Hello from vagrant-libvirt`, but this time it comes from the
   reverse proxy.
1. Party!

## Cleaning up

The VM can be torn down after playing around using `vagrant destroy`.

## License

BSD-3-Clause

## Author Information

I am Johannes Kastl, reachable via git@johannes-kastl.de
