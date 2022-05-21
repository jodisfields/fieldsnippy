# Infrastructure As Code ```Jodis Fields```

## Certificate Management 

```properties
resource "intersight_certificatemanagement_policy" "certificate1" {
description = "sample certificate"
name        = "certificate1"
organization {
    moid        = var.organization
    object_type = "organization.Organization"
}
certificates {
    certificate {
        pem_certificate = var.pem_certificate1
    }
    enabled    = true
    privatekey = var.privatekey
    }
}
variable "privatekey" {
    type        = string
    sensitive   = true
    description = "Private key base64 encoded value"
}
variable "pem_certificate1" {
    type        = string
    sensitive   = true
    description = "Pem certificate base64 encoded value"
}
```

## Cisco Advances Security Appliance (ASA)

### Access In ```sa_access_in_rules```

```properties
resource cis "sa_access_in_rules"   "foo" {
interface = "inside"
    rule {
        source              =   "x.x.x.x/32"
        destination         =   "x.x.x.x/25"
        destination_service =   "tcp/443"
    }
    rule {
        source              =   "x.x.x.x/24"
        source_service      =   "udp"
        destination         =   "x.x.x.x/32"
        destination_service =   "udp/53"
    }
    rule {
        source              =   "x.x.x.x/23"
        destination         =   "x.x.x.x/23"
        destination_service =   "icmp/0"
    }
}
```

### Access Out Rules ```(ciscoasa_access_out_rules)```

```properties
resource "ciscoasa_access_out_rules" "foo" {
  interface = "inside"
  rule {
    source              = "x.x.x.x/32"
    destination         = "x.x.x.x/25"
    destination_service = "tcp/443"
  }
  rule {
    source              = "x.x.x.x/24"
    source_service      = "udp"
    destination         = "x.x.x.x/32"
    destination_service = "udp/53"
  }
  rule {
    source              = "x.x.x.x/23"
    destination         = "x.x.x.x/23"
    destination_service = "icmp/0"
  }
}
```

### ACL ```(ciscoasa_acl)```

```properties
resource "ciscoasa_acl" "foo" {
  name = "aclname"
  rule {
    source              = "x.x.x.x/32"
    destination         = "x.x.x.x/25"
    destination_service = "tcp/443"
  }
  rule {
    source              = "x.x.x.x/24"
    source_service      = "udp"
    destination         = "x.x.x.x/32"
    destination_service = "udp/53"
  }
  rule {
    source              = "x.x.x.x/23"
    destination         = "x.x.x.x/23"
    destination_service = "icmp/0"
  }
}
```

### DHCP Relay Globalsettings ```(ciscoasa_dhcp_relay_globalsettings)```

```properties
resource "ciscoasa_dhcp_relay_globalsettings" "test" {
  ipv4_timeout              = 90
  ipv6_timeout              = 90
  trusted_on_all_interfaces = true
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "tengig_ipv4_dhcp" {
  name           = "tengig"
  hardware_id    = "TenGigabitEthernet0/2"
  interface_desc = "Interface DHCP"
  ip_address {
    dhcp {
      dhcp_option_using_mac = false
      dhcp_broadcast        = true
      dhcp_client {
        set_default_route = true
        metric            = 1
        primary_track_id  = -1
        tracking_enabled  = false
      }
    }
  }
  security_level = 0
}
```

### DHCP Relay Local ```(ciscoasa_dhcp_relay_local)```

```properties
resource "ciscoasa_dhcp_relay_local" "dhcp_relay_test" {
  interface = ciscoasa_interface_physical.tengig_ipv4_dhcp.name
  servers = [
    "x.x.x.x"
    "20.20.30.26",
    "x.x.x.x"
  ]
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "ipv4_interface" {
  name           = "test"
  hardware_id    = "TenGigabitEthernet0/1"
  interface_desc = "test descr"
  ip_address {
    static {
      ip       = "x.x.x.x"
      net_mask = "x.x.x.x"
    }
  }
  security_level = 0
}
```

### DHCP Server ```(ciscoasa_dhcp_server)```

```properties
resource "ciscoasa_dhcp_server" "dhcp_test" {
  interface             = ciscoasa_interface_physical.ipv4_interface.name
  enabled               = true
  pool_start_ip         = "x.x.x.x"
  pool_end_ip           = "x.x.x.x"
  dns_ip_primary        = "x.x.x.x"
  dns_ip_secondary      = "x.x.x.x"
  wins_ip_primary       = "x.x.x.x"
  wins_ip_secondary     = "x.x.x.x"
  lease_length          = "305"
  ping_timeout          = "40"
  domain_name           = "testing1"
  auto_config_enabled   = true
  auto_config_interface = false
  vpn_override          = false
  options {
    type   = "hex"
    code   = 2
    value1 = "c52f"
  }
  options {
    type   = "ascii"
    code   = 4
    value1 = "1261"
  }
  options {
    type   = "ip"
    code   = 13
    value1 = "x.x.x.x"
    value2 = "x.x.x.x"
  }
  ddns_update_dns_client        = true
  ddns_update_both_records      = true
  ddns_override_client_settings = true
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "ipv4_interface" {
  name           = "test"
  hardware_id    = "TenGigabitEthernet0/1"
  interface_desc = "test descr"
  ip_address {
    static {
      ip       = "x.x.x.x"
      net_mask = "x.x.x.x"
    }
  }
  security_level = 0
}
```

### DHCP Server ```(ciscoasa_dhcp_server)```

```properties
resource "ciscoasa_dhcp_server" "dhcp_test" {
  interface             = ciscoasa_interface_physical.ipv4_interface.name
  enabled               = true
  pool_start_ip         = "x.x.x.x"
  pool_end_ip           = "x.x.x.x"
  dns_ip_primary        = "x.x.x.x"
  dns_ip_secondary      = "x.x.x.x"
  wins_ip_primary       = "x.x.x.x"
  wins_ip_secondary     = "x.x.x.x"
  lease_length          = "305"
  ping_timeout          = "40"
  domain_name           = "testing1"
  auto_config_enabled   = true
  auto_config_interface = false
  vpn_override          = false
  options {
    type   = "hex"
    code   = 2
    value1 = "c52f"
  }
  options {
    type   = "ascii"
    code   = 4
    value1 = "1261"
  }
  options {
    type   = "ip"
    code   = 13
    value1 = "x.x.x.x"
    value2 = "x.x.x.x"
  }
  ddns_update_dns_client        = true
  ddns_update_both_records      = true
  ddns_override_client_settings = true
}
```

### Failover Setup ```(ciscoasa_failover_setup)```

```properties
resource "ciscoasa_failover_setup" "test" {
  enable                           = true
  lan_interface_hw_id              = "TenGigabitEthernet0/0"
  lan_failover_name                = "test-fo"
  lan_primary_ip                   = "x.x.x.x"
  lan_secondary_ip                 = "x.x.x.x"
  lan_net_mask                     = "x.x.x.x"
  lan_preferred_role               = "secondary"
  state_interface_hw_id            = "TenGigabitEthernet0/0"
  state_failover_name              = "test-fo"
  state_primary_ip                 = "x.x.x.x"
  state_secondary_ip               = "x.x.x.x"
  state_net_mask                   = "x.x.x.x"
  failed_interfaces_threshold      = "10"
  failed_interfaces_threshold_unit = "Percentage"
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "tengig_ipv4_static" {
  name           = "tengig0"
  hardware_id    = "TenGigabitEthernet0/0"
  interface_desc = "TenGig Static"
  ip_address {
    static {
      ip       = "10.0.1.5"
      net_mask = "x.x.x.x"
    }
  }
  security_level = 0
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "tengig_ipv4_dhcp" {
  name           = "tengig1"
  hardware_id    = "TenGigabitEthernet0/1"
  interface_desc = "TenGig DHCP"

  ip_address {
    dhcp {
      dhcp_option_using_mac = false
      dhcp_broadcast        = true
      dhcp_client {
        set_default_route = false
        metric            = 1
        primary_track_id  = 6
        tracking_enabled  = false
      }
    }
  }
  security_level = 0
}
```

### Interface Physical ```(ciscoasa_interface_physical)```

```properties
resource "ciscoasa_interface_physical" "tengig_ipv6" {
  name           = "tengig2"
  hardware_id    = "TenGigabitEthernet0/2"
  interface_desc = "TenGig Ipv6 with standby address"
  ipv6_info {
    auto_config   = false
    dad_attempts  = 1
    enabled       = true
    enforce_eui64 = false
    link_local_address {
      address = "fe80::202:b3ff:eef1:7234"
      standby = "fe80::202:b3ff"
    }
    ipv6_addresses {
      address       = "2001:db8:2222:1044::47"
      standby       = "2001:db8:2222:1044::46"
      prefix_length = 64
    }
    managed_address_config = true
    n_discovery_prefix_list {
      off_link           = false
      no_advertise       = true
      preferred_lifetime = 604800
      valid_lifetime     = 2592000
      has_duration       = true
      default_prefix     = true
    }
    ns_interval                 = 1000
    other_stateful_config       = true
    reachable_time              = 0
    router_advert_interval      = 250
    router_advert_interval_unit = "sec"
    router_advert_lifetime      = 1800
    suppress_router_advert      = true
  }
  security_level = 0
}
```

### Interface Vlan ```(ciscoasa_interface_vlan)```

```properties
resource "ciscoasa_interface_vlan" "tengig_no_config" {
  name           = "vlantengig140"
  hardware_id    = "TenGigabitEthernet0/0.140"
  interface_desc = "VlanTenGig Zero Conf"
  vlan_id        = 141
  security_level = 0
}
```

### Interface Vlan ```(ciscoasa_interface_vlan)```

```properties
resource "ciscoasa_interface_vlan" "tengig_ipv4_static" {
  name           = "vlantengig150"
  hardware_id    = "TenGigabitEthernet0/1.150"
  interface_desc = "VlanTenGig Static"
  ip_address {
    static {
      ip       = "10.0.2.6"
      net_mask = "x.x.x.x"
    }
  }
  security_level = 0
}
```

### Interface Vlan ```(ciscoasa_interface_vlan)```

```properties
resource "ciscoasa_interface_vlan" "tengig_ipv4_dhcp" {
  name           = "vlantengig160"
  hardware_id    = "TenGigabitEthernet0/1.160"
  interface_desc = "VlanTenGig DHCP"

  ip_address {
    dhcp {
      dhcp_option_using_mac = false
      dhcp_broadcast        = true
      dhcp_client {
        set_default_route = false
        metric            = 1
        primary_track_id  = 6
        tracking_enabled  = false
      }
    }
  }
  security_level = 0
}
```

### Interface Vlan ```(ciscoasa_interface_vlan)```

```properties
resource "ciscoasa_interface_vlan" "tengig_ipv6" {
  name           = "vlantengig170"
  hardware_id    = "TenGigabitEthernet0/2.170"
  interface_desc = "VlanTenGig Ipv6 with standby address"
  ipv6_info {
    auto_config   = false
    dad_attempts  = 1
    enabled       = true
    enforce_eui64 = false
    link_local_address {
      address = "fe80::20e:cff:fe3b:883c"
      standby = "fe80::20e:cff"
    }
    ipv6_addresses {
      address       = "2001:db8:a0b:12f0::47"
      standby       = "2001:db8:a0b:12f0::46"
      prefix_length = 64
    }
    managed_address_config = true
    n_discovery_prefix_list {
      off_link           = false
      no_advertise       = true
      preferred_lifetime = 604800
      valid_lifetime     = 2592000
      has_duration       = true
      default_prefix     = true
    }
    ns_interval                 = 1000
    other_stateful_config       = true
    reachable_time              = 0
    router_advert_interval      = 250
    router_advert_interval_unit = "sec"
    router_advert_lifetime      = 1800
    suppress_router_advert      = true
  }
  security_level = 0
}
```

### License Config ```(ciscoasa_license_config)```

```properties
resource "ciscoasa_license_config" "test" {
  throughput         = "2G"
}
```

### License Register ```(ciscoasa_license_register)```

```properties
resource "ciscoasa_license_register" "test" {
  id_token = "<registration token>"
}
```

### License Renewauth ```(ciscoasa_license_renewauth)```

```properties
resource "ciscoasa_license_renewauth" "test" {}
```

### NAT ```(ciscoasa_nat)```

```properties
resource "ciscoasa_nat" "auto_test" {
  section                   = "auto"
  description               = "static auto test"
  mode                      = "static"
  original_interface_name   = ciscoasa_interface_physical.inside.name
  translated_interface_name = ciscoasa_interface_physical.outside.name
  original_source_kind      = "objectRef#NetworkObj"
  original_source_value     = ciscoasa_network_object.inside_host.name
  translated_source_kind    = "objectRef#NetworkObj"
  translated_source_value   = ciscoasa_network_object.inside_host_translated.name
}
```
### NAT ```(ciscoasa_nat)```

```properties
resource "ciscoasa_nat" "after_test1" {
  active                     = false
  section                    = "after"
  description                = "dynamic after test 1"
  mode                       = "dynamic"
  position                   = 1
  original_interface_name    = ciscoasa_interface_physical.inside.name
  translated_interface_name  = ciscoasa_interface_physical.outside.name
  original_source_kind       = "objectRef#NetworkObj"
  original_source_value      = ciscoasa_network_object.inside_host.name
  original_destination_kind  = "objectRef#NetworkObj"
  original_destination_value = ciscoasa_network_object.outside_pool.name
  original_service_kind      = "objectRef#TcpUdpServiceObj"
  original_service_value     = ciscoasa_network_service.tcp_port.name
  translated_source_kind     = "objectRef#NetworkObj"
  translated_source_value    = ciscoasa_network_object.host_101.name
  translated_service_kind    = "objectRef#TcpUdpServiceObj"
  translated_service_value   = ciscoasa_network_service.tcp_dns.name
}
```

### Network Object ```(ciscoasa_network_object)```

```properties
resource "ciscoasa_network_object" "ipv4host" {
  name  = "ipv4_host"
  value = "x.x.x.x"
}```properties
resource "ciscoasa_network_object" "ipv4range" {
  name  = "ipv4_range"
  value = "x.x.x.x-y.y.y.y"
}```properties
resource "ciscoasa_network_object" "ipv4_subnet" {
  name  = "ipv4_subnet"
  value = "x.x.x.x/25"
}
```

### Network Object ```(ciscoasa_network_object)```

```properties
resource "ciscoasa_network_object" "ipv4host" {
  name  = "my_object"
  value = "x.x.x.x"
}
```

### Network Object Group ```(ciscoasa_network_object_group)```

```properties
resource "ciscoasa_network_object_group" "objgrp_mixed" {
  name = "my_group"

  members = [
    "${ciscoasa_network_object.obj_ipv4host.name}",
    "x.x.x.x",
      "10.5.10.0/24",
  ]
}
```

### Network Service ```(ciscoasa_network_service)```

```properties
resource "ciscoasa_network_service" "tcp-port" {
  name  = "tcp-port"
  value = "tcp/800"
}```properties
resource "ciscoasa_network_service" "tcp-with-source" {
  name  = "tcp-with-source"
  value = "tcp/https,source=7000-8000"
}```properties
resource "ciscoasa_network_service" "icmp4-with-type-code" {
  name  = "icmp4-with-type-code"
  value = "icmp/50/60"
}
```

### Network Service Group ```(ciscoasa_network_service_group)```

```properties
resource "ciscoasa_network_service_group" "service_group" {
  name = "service_group"

  members = [
    "tcp/80",
    "udp/53",
    "tcp/6001-6500",
    "icmp/0",
  ]
}
```

### Network Service Group ```(ciscoasa_network_service_group)```

```properties
resource "ciscoasa_network_service_group" "service_group" {
  name = "service_group"

  members = [
    "tcp/80",
    "udp/53",
    "tcp/6001-6500",
    "icmp/0",
  ]
}
```

### Static Route ```(ciscoasa_static_route)```

```properties
resource "ciscoasa_static_route" "ipv4_static_route" {
  interface = "inside"
  network   = "x.x.x.x/x"
  gateway   = "x.x.x.x"
}
```

### Static Route ```(ciscoasa_static_route)```

```properties
resource "ciscoasa_static_route" "ipv6_static_route" {
  interface = "inside"
  network   = "x.x.x.x/x"
  gateway   = "x.x.x.x"
}
```

### Timerange ```(ciscoasa_timerange)```

```properties
resource "ciscoasa_timerange" "tr" {
  name = "tr"
  value {
    start = "now"
    end   = "03:47 May 14 2025"
    periodic {
      frequency    = "Wednesday to Thursday"
      start_hour   = 4
      start_minute = 3
      end_hour     = 23
      end_minute   = 59
    }
  }
}
```

### Static Route ```(ciscoasa_static_route)```

```properties
resource "ciscoasa_static_route" "ipv4_static_route" {
  interface = "inside"
  network   = "x.x.x.x/x"
  gateway   = "x.x.x.x"
}
```

### Write Memory ```(ciscoasa_write_memory)```

```properties
resource "ciscoasa_write_memory" "write_memory" { 
triggers = {
    ciscoasa_static_route.ipv4_static_route = propertiesonencode(ciscoasa_static_route.ipv4_static_route)
}
```

## Cisco Identity Services Engine (ISE)

### ```ciscoise_aci_settings```

```

```

### ```ciscoise_active_directory```

```

```

### ```ciscoise_allowed_protocols```

```

```

### ```ciscoise_anc_endpoint```

```

```

### ```ciscoise_anc_policy```

```

```

### ```ciscoise_authorization_profile```

```

```

### ```ciscoise_byod_portal```

```

```

### ```ciscoise_certificate_profile```

```

```

### ```ciscoise_device_administration_authentication_rules```

```

```

### ```ciscoise_device_administration_authorization_rules```

```

```

### ```ciscoise_device_administration_conditions```

```

```

### ```ciscoise_device_administration_global_exception_rules```

```

```

### ```ciscoise_device_administration_local_exception_rules```

```

```

### ```ciscoise_device_administration_network_conditions```

```

```

### ```ciscoise_device_administration_policy_set```

```

```

### ```ciscoise_device_administration_time_date_conditions```

```

```

### ```ciscoise_downloadable_acl```

```

```

### ```ciscoise_egress_matrix_cell```

```

```

### ```ciscoise_endpoint```

```

```

### ```ciscoise_endpoint_group```

```

```

### ```ciscoise_external_radius_server```

```

```

### ```ciscoise_filter_policy```

```

```

### ```ciscoise_guest_smtp_notification_settings```

```

```

### ```ciscoise_guest_ssid```

```

```

### ```ciscoise_guest_type```

```

```

### ```ciscoise_guest_user```

```

```

### ```ciscoise_hotspot_portal```

```

```

### ```ciscoise_id_store_sequence```

```

```

### ```ciscoise_identity_group```

```

```

### ```ciscoise_internal_user```

```

```

### ```ciscoise_licensing_registration```

```

```

### ```ciscoise_licensing_tier_state```

```

```

### ```ciscoise_my_device_portal```

```

```

### ```ciscoise_native_supplicant_profile```

```

```

### ```ciscoise_network_access_authentication_rules```

```

```

### ```ciscoise_network_access_authorization_rules```

```

```

### ```ciscoise_network_access_conditions```

```

```

### ```ciscoise_network_access_dictionary```

```

```

### ```ciscoise_network_access_dictionary_attribute```

```

```

### ```ciscoise_network_access_global_exception_rules```

```

```

### ```ciscoise_network_access_local_exception_rules```

```

```

### ```ciscoise_network_access_network_condition```

```

```

### ```ciscoise_network_access_policy_set```

```

```

### ```ciscoise_network_access_time_date_conditions```

```

```

### ```ciscoise_network_device```

```

```

### ```ciscoise_network_device_group```

```

```

### ```ciscoise_node_deployment```

```

```

### ```ciscoise_node_group```

```

```

### ```ciscoise_node_group_node```

```

```

### ```ciscoise_node_services_profiler_probe_config```

```

```

### ```ciscoise_node_services_sxp_interfaces```

```

```

### ```ciscoise_pan_ha```

```

```

### ```ciscoise_portal_global_setting```

```

```

### ```ciscoise_portal_theme```

```

```

### ```ciscoise_proxy_connection_settings```

```

```

### ```ciscoise_px_grid_node```

```

```

### ```ciscoise_radius_server_sequence```

```

```

### ```ciscoise_repository```

```

```

### ```ciscoise_rest_id_store```

```

```

### ```ciscoise_self_registered_portal```

```

```

### ```ciscoise_sg_acl```

```

```

### ```ciscoise_sg_mapping```

```

```

### ```ciscoise_sg_mapping_group```

```

```

### ```ciscoise_sg_to_vn_to_vlan```

```

```

### ```ciscoise_sgt```

```

```

### ```ciscoise_sponsor_group```

```

```

### ```ciscoise_sponsor_portal```

```

```

### ```ciscoise_sponsored_guest_portal```

```

```

### ```ciscoise_sxp_connections```

```

```

### ```ciscoise_sxp_local_bindings```

```

```

### ```ciscoise_sxp_vpns```

```

```

### ```ciscoise_system_certificate```

```

```

### ```ciscoise_tacacs_command_sets```

```

```

### ```ciscoise_tacacs_external_servers```

```

```

### ```ciscoise_tacacs_profile```

```

```

### ```ciscoise_tacacs_server_sequence```

```

```

### ```ciscoise_transport_gateway_settings```

```

```

### ```ciscoise_trusted_certificate```

```

```

### ```ciscoise_trustsec_nbar_app```

```

```

### ```ciscoise_trustsec_sg_vn_mapping```

```

```

### ```ciscoise_trustsec_vn```

```

```

### ```ciscoise_trustsec_vn_vlan_mapping```

```

```
