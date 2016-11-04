= Issue

From what I recall, when after we established a VPN to AWS, we were able to resolve hostnames like `ip-172-xx-xx-xx.us-west-2.compute.internal` from machines on our LAN.

Then, one day, this seemed to stop.  Working.  Maybe it never worked in the first place.  We weren't able to directly use the DNS server on the AWS side any more.

But it was a feature we needed on the network, at least for machines that were intended to query our Hadoop cluster that lived on AWS.  We needed to be able to resolve those specific hostnames because we were testing Kerberos and Kerberos seems to be picky about what hostnames it will accept when authenticating.

= Solution

Turns out the solution that was quickest to implement was to turn one of our perma-instances on AWS into a forwarding DNS server.  A forwarding DNS server seems to simply take a DNS query and turns around and asks another DNS server to resolve the query, then passes the answer back.

So, we could use the perma-instance as a DNS and the perma-instance would forward the query to the DNS on AWS's side that we could no longer see.

Here are some links I found on Google:

- https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-caching-or-forwarding-dns-server-on-ubuntu-14-04
- https://www.centos.org/forums/viewtopic.php?t=46103
- https://aws.amazon.com/blogs/security/how-to-set-up-dns-resolution-between-on-premises-networks-and-aws-using-aws-directory-service-and-amazon-route-53/
- https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-centos-7

Ultimately I created a config file at /etc/named.conf on our Centos 7.2 perma-instance
