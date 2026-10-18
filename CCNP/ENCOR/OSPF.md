# OSPF

> [!NOTE]
> EIGRP is distant vector routing protocol, while OSPF is a link state routing protocol.

![[screenshot_2026-10-06_18-50-40.png|800]]

![[OSPF LSA Flooding Infographic.png|800]]

> Every time a link goes down, whole topology in area must be recalculated, and this calculation uses too much CPU, so if we have flat OSPF, the CPU usage on routers can go very high.
> Each Area has its own topology database, routing table.

![[screenshot_2026-10-06_19-05-53.png|800]]
![[screenshot_2026-10-06_19-10-44.png|800]]
![[screenshot_2026-10-06_19-14-31.png|800]]
![[screenshot_2026-10-06_19-15-27.png|800]]

> to see OSPF interface info: `show ip ospf interface <int-name>`

## Link State VS Distant Vector

| Feature               | Distance Vector                                                    | Link State                          |
| --------------------- | ------------------------------------------------------------------ | ----------------------------------- |
| Basic idea            | Learns distance + direction to destinations                        | Learns the topology of the network  |
| Knowledge             | Usually knows routes via neighbors                                 | Builds a map of the network area    |
| Information exchanged | Routing information / distance metrics                             | Link-state information (LSAs)       |
| Algorithm             | Bellman-Ford / variants                                            | Dijkstra SPF                        |
| Updates               | Traditionally periodic; modern protocols may use triggered updates | Primarily event-driven              |
| Convergence           | Generally slower                                                   | Generally faster                    |
| CPU/RAM               | Lower requirements                                                 | Higher requirements                 |
| Network scalability   | Lower                                                              | Higher                              |
| Loop risk             | Historically higher                                                | Lower, though loops can still occur |
| Examples              | RIP, EIGRP*                                                        | OSPF, IS-IS                         |

> EIGRP is technically a hybrid/advanced distance-vector protocol, commonly described as an advanced distance-vector protocol rather than pure distance vector.
