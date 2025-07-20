#### **LIST ALL OWNED USERS**

`MATCH (m:User) WHERE m.owned=TRUE RETURN m`

#### **LIST ALL OWNED COMPUTERS**

`MATCH (m:Computer) WHERE m.owned=TRUE RETURN m`

#### **LIST ALL OWNED GROUPS**

`MATCH (m:Group) WHERE m.owned=TRUE RETURN m`

#### **LIST ALL HIGH VALUED TARGETS**

`MATCH (m) WHERE m.highvalue=TRUE RETURN m`

#### **LIST THE GROUPS OF ALL OWNED USERS**

`MATCH (m:User) WHERE m.owned=TRUE WITH m MATCH p=(m)-[:MemberOf*1..]->(n:Group) RETURN p`

#### **FIND ALL KERBEROASTABLE USERS**

`MATCH (n:User) WHERE n.hasspn=true RETURN n`

#### **FIND USERS WITH SPN SET < 5 YEARS**

`MATCH (u:User) WHERE u.hasspn=true AND u.pwdlastset < (datetime().epochseconds - (5 * 365.25 * 86400)) AND NOT u.pwdlastset IN [-1.0, 0.0] RETURN u.name, u.pwdlastset ORDER BY u.pwdlastset`

#### **FIND KERBEROASTABLE USERS WITH DA PATH**

`MATCH (u:User {hasspn:true}) MATCH (g:Group) WHERE g.objectid ENDS WITH '-512' MATCH p = shortestPath( (u)-[*1..]->(g) ) RETURN p`

#### **FIND MACHINES DOMAIN USERS CAN RDP INTO**

`MATCH p=(g:Group)-[:CanRDP]->(c:Computer) WHERE g.objectid ENDS WITH '-513' RETURN p`

#### **FIND GROUPS THAT CAN RESET PASSWORDS**

`MATCH p=(m:Group)-[r:ForceChangePassword]->(n:User) RETURN p`

#### **FIND GROUPS WITH LOCAL ADMIN RIGHTS**

`MATCH p=(m:Group)-[r:AdminTo]->(n:Computer) RETURN p`

#### **FIND ALL ACTIVE DOMAIN ADMIN SESSIONS**

`MATCH (n:User)-[:MemberOf]->(g:Group) WHERE g.objectid ENDS WITH '-512' MATCH p = (c:Computer)-[:HasSession]->(n) RETURN p`

#### **FIND COMPUTERS WITH UNCONSTRAINED DELEGATION**

`MATCH (c:Computer {unconstraineddelegation:true}) RETURN c`

#### **FIND COMPUTERS WITH UNSUPPORTED OS**

`MATCH (H) WHERE H.operatingsystem =~ '.*(2000'`

#### **FIND USERS LOGGED IN WITHIN 90 DAYS**

`MATCH (u:User) WHERE u.lastlogon > (datetime().epochseconds - (90 * 86400)) RETURN u`

#### **FIND USERS WITH PASSWORDS SET WITHIN 90 DAYS**

`MATCH (u:User) WHERE u.pwdlastset > (datetime().epochseconds - (90 * 86400)) RETURN u`

#### **FIND CONSTRAINED DELEGATION**

`MATCH p=(u:User)-[:AllowedToDelegate]->(c:Computer) RETURN p`

#### **FIND MSSQL COMPUTERS**

`MATCH (c:Computer) WHERE ANY (x IN c.serviceprincipalnames WHERE toUpper(x) CONTAINS 'MSSQL') RETURN c`

#### **VIEW ALL GPOS**

`MATCH (n:GPO) RETURN n`

#### **VIEW ALL ADMIN GROUPS**

`MATCH (n:Group) WHERE toUpper(n.name) CONTAINS 'ADMIN' RETURN n`

#### **FIND AS-REP ROASTABLE USERS**

`MATCH (u:User {dontreqpreauth: true}) RETURN u`

#### **SHOW HIGH VALUE TARGET GROUPS**

`MATCH p=(n:User)-[r:MemberOf*1..]->(m:Group {highvalue:true}) RETURN p`

#### **FIND GROUPS WITH BOTH USERS AND COMPUTERS**

`MATCH (c:Computer)-[r:MemberOf*1..]->(groupsWithComps:Group) WITH groupsWithComps MATCH (u:User)-[r:MemberOf*1..]->(groupsWithComps) RETURN DISTINCT(groupsWithComps) AS groupsWithCompsAndUsers`

#### **FIND KERBEROASTABLE USERS IN HIGH VALUE GROUPS**

`MATCH (u:User)-[r:MemberOf*1..]->(g:Group) WHERE g.highvalue=true AND u.hasspn=true RETURN u`

#### **FIND KERBEROASTABLE USERS AND ADMINTO**

`MATCH (u:User {hasspn:true}) OPTIONAL MATCH (u)-[r:AdminTo]->(c:Computer) RETURN u`

#### **FIND CONSTRAINED DELEGATION PERMISSIONS**

`MATCH (c:Computer) WHERE c.allowedtodelegate IS NOT NULL RETURN c`

#### **FIND DOMAIN USER PERMISSIONS ON GPO**

`MATCH p=(u:User)-[r:AllExtendedRights`

#### **FIND UNPRIVILEGED USER GROUP ADD RIGHTS**

`MATCH (n:User {admincount:False}) MATCH p=allShortestPaths((n)-[r:AddMember*1..]->(m:Group)) RETURN p`

#### **FIND VPN GROUP MEMBERS**

`MATCH p=(u:User)-[:MemberOf]->(g:Group) WHERE toUPPER(g.name) CONTAINS 'VPN' RETURN p`

#### **FIND ACTIVE USERS NEVER LOGGED ON**

`MATCH (n:User) WHERE n.lastlogontimestamp=-1.0 AND n.enabled=TRUE RETURN n`

#### **FIND CROSS-DOMAIN PERMISSIONS**

`MATCH p=(n)-[r]->(m) WHERE NOT n.domain = m.domain RETURN p`

#### **FIND USER SESSIONS IN A SPECIFIC DOMAIN**

`MATCH p=(m:Computer)-[r:HasSession]->(n:User {domain:'specificDomainValue'}) RETURN p`