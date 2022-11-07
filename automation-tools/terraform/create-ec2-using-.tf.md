# Project

**We are adding this script in our main.tf to add VM**

<pre><code>	module "hostname" {
	source         = "../../../modules/default-server"
	vm_name        = "hostname"
	vm_image_id    = "ID"
	vm_network_id  = NW-ID
	 vm_ip          = "IP_address"
<strong>	available_zone = var.available_zone</strong></code></pre>
