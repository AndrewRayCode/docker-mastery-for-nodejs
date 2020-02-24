# Local DNS and many ports issues

- Can use x.localhost, y.localhost, but is chrome feature, won't work with
  command line tools and maybe not other browsers
- Can use "wildcard domain" and can set up one host in hostsfile, like *.vcap.me
  (a vmware domain), and develop that way.
- can use "dnsmasq" on mac/linux, and send dns to wherever you want, and answers
  dns requests. more setup process, runs on host (not in container)
- last resort is hosts file
  - need local admin
  - doesn't work with admins, need separate entry for each microservice
  - there are scripts/tools, but not within docker workflow


