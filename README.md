#Riak puppet module (For rhel/centos)


##Just do

      vagrant up
      
On project root, and it should automagically setup Riak cluster with 3 virtual servers:

      10.0.3.10 (will have Riak Control installed)
      10.0.3.11
      10.0.3.11

You can test it with: 
      
      curl -v 10.0.3.10:8098/riak/ping

Or test Riak Control on node riak1 by going with your browser to: https://10.0.3.10:8068 
and logging in with user/pass (it will whine about invalid certificate)

Example:

      node "riak1", "riak2", "riak3" {
        exec { "purge default firewall":
          command => "/sbin/iptables --flush",
          user    => 'root',
        }
        if $::fqdn == "riak1.vagrant.local" {
          class { "riak": riak_control => true}
        } else {
          class { "riak": join_ip => "10.0.3.10" }
        }
      }
