## 8 node topology

### options

0. Do nothing, keep growing level-2 and hope for the best

1. Introduce ISIS level-1 at the PE layer - Q for Peter on convergence
   * ISIS level-2 core, level-1 DCI layer
   * ISIS attachment bit set at P level-1-2 DCI nodes: leaks default route from L2 into DCI L1
   * Pros: BGP-free P nodes, PE peers with RR, very common scaled design, can support IPFRR - need to test
   * Cons: leaking level-2 routes into level-1 - no problem at DCI, but could be a problem if Internet peering nodes are level-1
  
2. Run a separate ISIS process from P to PE, then iBGP-SR over that - Bruce to POC
   * Seamless MPLS design
   * Pros: common scaled design
   * Cons: P nodes as inline RRs for Core loopback reachablity, but PE will also need iBGP session with RR (recursive via P nodes) for Internet routes. No FRR

3. SR iBGP-LU from P to PE with peering over loopbacks via static routes - not good for scaled network
   * A static route version of seamless MPLS: P acts as inline route-reflector
   * Pros: it should work
   * Cons: P nodes as inline RRs for Core loopback reachablity, but PE will also need iBGP session with RR (recursive via P nodes) for Internet routes. And the use of static routes. No FRR

4. SR eBGP-LU from P to PE - probably breaks current flat ASN 8075
   * Inter-AS option-B style
   * P-Nodes do not know BGP Node-SIDs of another domain
   * Pros: common design, seen in other hyperscalers
   * Cons: PE will need additional eBGP-multihop peering session to RR for Internet routes. No FRR
  
5. Divide WAN into geographic domains (NA, EU, APJ)
   * New ASBR peering layer, or convert ASBR P nodes
   * Need new BGP ASNs for geos - probably a non-starter
  
6. Some sort of ASBR asn-override, but must hide/overwrite source ASN

7. Converged SDN Transport (ISIS ASBRs)
   * P node as ASBR between PE and Core
   * Geo-ASBRs

### SRv6 Options

