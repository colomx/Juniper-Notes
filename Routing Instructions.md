### Routing Instructions

**These steps must be on internal VPN network**

QFX SSH 104.255.32.2

TOR SSH 100.100.(SWITCHID).0

#### 1. Assign new VLANID to client

Login to QFX and run command:

<pre>
show interfaces irb unit ?
</pre>

Find next increment of 1xxx vlan, then update Blesta client vlan info (Edit Client Profile).

#### 2. IP Assignment

Assign a IP block from the pool.

Fill in the following fields

Description: Client_XXXX Service_XXXX

Order ID: (service_id_goes_here)

Client ID: (client_id goes_here)

VLAN: (client_vlan_goes_here)


#### 3. QFX Configuration

**Add IP address**

<pre>
set interfaces irb unit (vlanid) family inet address (prefix gateway)/(prefix length)
</pre>

example: set interfaces irb unit 999 family inet address 1.0.0.1/24

**Add Client Description**

<pre>
set interfaces irb unit (vlanid) description Client_XXXX
</pre>

**Add VLAN**
<pre>
set vlan VLANXXXX vlan-id XXXX l3-interface irb.XXXX

set vlan VLANXXXX description "Client_XXXX"
</pre>

**Save changes**
<pre>
commit
</pre>

(check for errors after running command)

#### 4. EX TOR Configuration

<pre>
set vlan VLANXXXX vlan-id XXXX interface ge-0/0/(PORT #).0

set vlan VLANXXXX description "Client_XXXX"

set interface ge-0/0/x description "Client_XXXX Service_XXXX"
</pre>

Save changes:

<pre>
commit
</pre>

(check for errors after running command)

#### 5. NOC-PS Configuration

Add subnet/prefix to noc-ps

#### 6. Google Racks Document

Update VLAN

Update Prefix

-------------------------------------------------

Remove the switch port from vlan:

<pre>
delete vlan VLAN1099 interface ge-0/0/1.0
</pre>

--

Set Port Cap:

<pre>
set interfaces ge-0/0/0 ether-options speed 100m	
</pre>

Find out IP belongs to which VLAN:

<pre>
run show route x.x.x.x
</pre>
