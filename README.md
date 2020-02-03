# cme-wmi

Experimental plugin for CrackMapExec that adds a new protocol based on pure WMI : all of CrackMapExec's traffic passes via DCOM (TCP/135 and dynamic ports). 

Remote WMI access must be configured on the target. No SMB services are needed. CrackMapExec's HTTP server is not used. All command output is obtained exclusively using the WMI access, through storage in the WMI repository.

Currently only tested with CrackMapExec's Mimikatz module. 

## Usage

```
cme wmi targets.txt -d <DOMAIN> -u <USER> -p <PASS> [-H <HASH>] -M mimikatz
```
![usage](https://github.com/Orange-Cyberdefense/cme-wmi/blob/master/example.png)

As shown, commands are sent only via DCOM (TCP/135).

## Install

```
git clone https://github.com/byt3bl33d3r/CrackMapExec.git
git clone https://github.com/Orange-Cyberdefense/cme-wmi.git
cp -r cme-wmi/wmi CrackMapExec/cme/protocols
cp cme-wmi/wmi.py CrackMapExec/cme/protocols
sed -ri "s/supported_protocols = \[(.*)\]/supported_protocols = \[\1, 'wmi'\]/" CrackMapExec/cme/modules/mimikatz.py #adding the new 'wmi' protocol manually to the list of supported protocols of the mimikatz.py module
```

Rebuild CrackMapExec afterwards.

## TODO

- add --sam and --lsa options (for secrets an idea is using nishang's Get-LsaSecret as a new module instead of Invoke-Mimikatz)
- fix loggging error that makes mimikatz fail when using options -o COMMAND (works if manually modified in the mimikatz.py)
- add enum_host() function
- port to Python3 ;)
