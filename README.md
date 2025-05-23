STP Security

1. RSTP
Set on all switches:
Switch(config)#spanning-tree mode rapid-pvst

2. Access + Portfast + BPDU guard
Switch(config)#interface FastEthernet 0/1
Switch(config-if)#spanning-tree portfast
Switch(config-if)#spanning-tree bpduguard enable
In this exercise, we only have VLAN 1 – which is the default VLAN – so no need for additional
configuration.

3. Trunk
Switch(config)#interface FastEthernet 0/24
Switch(config-if)#switchport mode trunk

4. Root bridge
Configure Switch1 to be Root bridge:
Switch(config)#spanning-tree vlan 1 root primary

5. Root guard
Root guard osposobiti na svičevima Switch0 i Switch1, na portovima koji vode prema Access sloju:
Switch(config)#interface range FastEthernet 0/23-24
Switch(config-if-range)#spanning-tree guard root

6. Test
To see the active interfaces and their roles, use the following command:
Switch(config)#show spanning-tree active

Checking the BPDU guard configuration:
1. Add a new switch to the existing topology.
2. Connect the new switch to Switch3, port FastEthernet 0/1 (instead of PC A).

3. Notice that the link gets blocked.
4. Enter the following commands to check on Switch3 interfaces:
Switch#show interfaces FastEthernet 0/1
5. In the printout, notice “err-disabled” in the first line.
6. Return to the previous setup – connect PC A back to Switch3.
7. The link will stay inactive and this is due to port being switched off so it doesn’t notice any changes.
To enable the port again, use the following commands:
Switch(config-if)#shutdown
Switch(config-if)#no shutdown

Root guard test:
1. Configure Switch3 to become Root bridge – use the following command to check the current Root
bridge priority:
Switch#show spanning-tree detail

3. We are interested in the information printed in 4th row:
“Current root has priority 24577”.

4. Now that we know what the current Root bridge priority is, we can set a lower priority on Switch3:
Switch(config)#spanning-tree vlan 1 priority 20480

5. If we open Switch1’s CLI we will see the following message:
“%SPANTREE-2-ROOTGUARDBLOCK: Port 0/24 tried to become non-designated
in VLAN 1.”

6. Printout of the following command will show that Switch1 remains root in spite of having a lower
priority than Switch3:
Switch#show spanning-tree active
7. In the above’s command printout, we can also see that FastEthernet 0/24 is in BLK (blocked) state.
