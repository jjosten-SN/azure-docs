---
title: 'Azure VPN Client optional configuration steps: OpenVPN protocol - Windows'
titleSuffix: Azure Virtual WAN
description: Learn how to configure the Azure VPN Client optional configuration parameters for P2S OpenVPN connections.
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 07/06/2022
ms.author: cherylmc

---
# Configure Azure VPN Client optional settings - OpenVPN protocol - Windows

This article helps you configure optional settings for an Azure VPN Client installed on a Windows computer.

* For information about installing the Azure VPN Client, see [Configure the Azure VPN client - Windows](openvpn-azure-ad-client.md).

* For information about how to download VPN client profile configuration file (xml file), see [Download a global or hub-based profile](global-hub-profile.md).

> [!NOTE]
> The Azure VPN Client is only supported for OpenVPN® protocol connections.
>

## <a name="xml"></a>Edit and import VPN client profile configuration files

The steps in this article require you to modify and import the Azure VPN Client profile configuration file. To work with VPN client profile configuration files (xml files), do the following:

1. Locate the profile configuration file and open it using the editor of your choice.
1. Modify the file as necessary, then save your changes.
1. Import the file to configure the Azure VPN client.

You can import the file using these methods:

* Import using the Azure VPN Client interface. Open the Azure VPN Client and click **+** and then **Import**. Locate the modified xml file, configure any additional settings in the Azure VPN Client interface (if necessary), then click **Save**.

* Import the profile from a command-line prompt. Add the downloaded **azurevpnconfig.xml** file to the **%userprofile%\AppData\Local\Packages\Microsoft.AzureVpn_8wekyb3d8bbwe\LocalState** folder, then run the following command. To force the import, use the **-f** switch.

   ```xml
   azurevpn -i azurevpnconfig.xml 
   ```

## DNS

### <a name="add-suffix"></a>Add DNS suffixes

Modify the downloaded profile xml file and add the **\<dnssuffixes>\<dnssufix> \</dnssufix>\</dnssuffixes>** tags.

```xml
<azvpnprofile>
<clientconfig>

    <dnssuffixes>
          <dnssuffix>.mycorp.com</dnssuffix>
          <dnssuffix>.xyz.com</dnssuffix>
          <dnssuffix>.etc.net</dnssuffix>
    </dnssuffixes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="custom-dns"></a>Add custom DNS servers

Modify the downloaded profile xml file and add the **\<dnsservers>\<dnsserver> \</dnsserver>\</dnsservers>** tags.

```xml
<azvpnprofile>
<clientconfig>

  <dnsservers>
    <dnsserver>x.x.x.x</dnsserver>
        <dnsserver>y.y.y.y</dnsserver>
  </dnsservers>
    
</clientconfig>
</azvpnprofile>
```

> [!NOTE]
> The OpenVPN Azure AD client utilizes DNS Name Resolution Policy Table (NRPT) entries, which means DNS servers will not be listed under the output of `ipconfig /all`. To confirm your in-use DNS settings, please consult [Get-DnsClientNrptPolicy](/powershell/module/dnsclient/get-dnsclientnrptpolicy) in PowerShell.
>

## Routing

### <a name="custom-routes"></a>Add custom routes

Modify the downloaded profile xml file and add the **\<includeroutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</includeroutes>** tags.

```xml
<azvpnprofile>
<clientconfig>

  <includeroutes>
    <route>
      <destination>x.x.x.x</destination><mask>24</mask>
    </route>
  </includeroutes>
    
</clientconfig>
</azvpnprofile>
```

### <a name="forced-tunneling"></a>Direct all traffic to the VPN tunnel (force tunnel)

You can include 0/0 if you're using the Azure VPN Client version 2.1900:39.0 or higher.

Modify the downloaded profile xml file and add the **\<includeroutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</includeroutes>** tags. Make sure to update the version number to **2**.

```xml
<azvpnprofile>
<clientconfig>
  <includeroutes>
    <route>
      <destination>0.0.0.0</destination><mask>0</mask>
    </route>
  </includeroutes>
    </clientconfig>

<version>2</version>
</azvpnprofile>
```

### <a name="exclude-routes"></a>Block (exclude) routes

Modify the downloaded profile xml file and add the **\<excluderoutes>\<route>\<destination>\<mask> \</destination>\</mask>\</route>\</excluderoutes>** tags.

```xml
<azvpnprofile>
<clientconfig>

  <excluderoutes>
    <route>
      <destination>x.x.x.x</destination><mask>24</mask>
    </route>
  </excluderoutes>
    
</clientconfig>
</azvpnprofile>
```

## Certificates

This section applies to certificate authentication clients.

### <a name="multi-cert"></a>Specify multiple certificates

If you have 2 hubs for your virtual WAN that are each configured for P2S User VPN and use the same VPN configuration, and that configuration is configured to use multiple certificates, you can now configure the VPN clients for multiple certificates. This means that if one certificate can't be used for any reason, the other certificate can still be used for authentication. Previously, you couldn't configure the client with the settings for both certificates.

You can configure multiple certificate support on the client side by either using the Azure VPN Client interface (version 2.1963.44.0 or higher), or by modifying the xml profile to include multiple certificate tags.

You need to first download the User VPN profile in order to obtain the necessary settings. Go to the **Virtual WAN -> User VPN configurations** page. Select the User VPN configuration used by both hubs, then select **Download virtual WAN user VPN profile** to download the global user VPN profile (rather than the hub profile). The files you download contain the root end certificates.

* To configure multiple certificates directly in the Azure VPN Client, specify multiple certificates when you add a VPN connection.

* To configure the Azure VPN client using the profile xml file, modify the xml file to include multiple certificates, then import the file either directly in the Azure VPN client, or from the command line.

   ```xml
     </protocolconfig>
     <serverlist>
       <ServerEntry>
         <displayname
           i:nil="true" />
         <fqdn>wan.kycyz81dpw483xnf3fg62v24f.vpn.azure.com</fqdn>
       </ServerEntry>
     </serverlist>
     <servervalidation>
       <cert>
         <hash>A8985D3A65E5E5C4B2D7D66D40C6DD2FB19C5436</hash>
         <issuer
           i:nil="true" />
       </cert>
       <cert>
         <hash>59470697201baejC4B2D7D66D40C6DD2FB19C5436</hash>
         <issuer
           i:nil="true" />
       </cert>
       <cert>
         <hash>cab20a7f63f00f2bae76202gdfe36db3a03a9cb9</hash>
         <issuer
           i:nil="true" />
       </cert>
   ```

## Next steps

For more information, see [Create an Azure Active Directory tenant for P2S Open VPN connections that use Azure AD authentication](openvpn-azure-ad-tenant.md).
