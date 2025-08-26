---
legal-banner: true
---

### **List All Owned Users**
```cypher
MATCH (m:User) WHERE m.owned=TRUE RETURN m
```

### **List All Owned Computers**
```cypher
MATCH (m:Computer) WHERE m.owned=TRUE RETURN m
```

### **List All Owned Groups**
```cypher
MATCH (m:Group) WHERE m.owned=TRUE RETURN m
```

### **List All High Value Targets**
```cypher
MATCH (m) WHERE m.highvalue=TRUE RETURN m
```

### **List the Groups of All Owned Users**
```cypher
MATCH (m:User) WHERE m.owned=TRUE 
WITH m 
MATCH p=(m)-[:MemberOf*1..]->(n:Group) 
RETURN p
```

### **Find All Kerberoastable Users**
```cypher
MATCH (n:User) WHERE n.hasspn=true RETURN n
```

### **Find Users with SPN Set Older Than 5 Years**
```cypher
MATCH (u:User) 
WHERE u.hasspn=true 
AND u.pwdlastset < (datetime().epochseconds - (5 * 365.25 * 86400)) 
AND NOT u.pwdlastset IN [-1.0, 0.0] 
RETURN u.name, u.pwdlastset 
ORDER BY u.pwdlastset
```

### **Find Kerberoastable Users with DA Path**
```cypher
MATCH (u:User {hasspn:true}) 
MATCH (g:Group) WHERE g.objectid ENDS WITH '-512' 
MATCH p = shortestPath((u)-[*1..]->(g)) 
RETURN p
```

### **Find Machines Domain Users Can RDP Into**
```cypher
MATCH p=(g:Group)-[:CanRDP]->(c:Computer) 
WHERE g.objectid ENDS WITH '-513' 
RETURN p
```

### **Find Groups That Can Reset Passwords**
```cypher
MATCH p=(m:Group)-[r:ForceChangePassword]->(n:User) RETURN p
```

### **Find Groups with Local Admin Rights**
```cypher
MATCH p=(m:Group)-[r:AdminTo]->(n:Computer) RETURN p
```

### **Find All Active Domain Admin Sessions**
```cypher
MATCH (n:User)-[:MemberOf]->(g:Group) 
WHERE g.objectid ENDS WITH '-512' 
MATCH p = (c:Computer)-[:HasSession]->(n) 
RETURN p
```

### **Find Computers with Unconstrained Delegation**
```cypher
MATCH (c:Computer {unconstraineddelegation:true}) RETURN c
```

### **Find Computers with Unsupported OS**
```cypher
MATCH (H) WHERE H.operatingsystem =~ '.*(2000'
```

### **Find Users Logged In Within 90 Days**
```cypher
MATCH (u:User) 
WHERE u.lastlogon > (datetime().epochseconds - (90 * 86400)) 
RETURN u
```

### **Find Users with Passwords Set Within 90 Days**
```cypher
MATCH (u:User) 
WHERE u.pwdlastset > (datetime().epochseconds - (90 * 86400)) 
RETURN u
```

### **Find Constrained Delegation**
```cypher
MATCH p=(u:User)-[:AllowedToDelegate]->(c:Computer) RETURN p
```

### **Find MSSQL Computers**
```cypher
MATCH (c:Computer) 
WHERE ANY (x IN c.serviceprincipalnames WHERE toUpper(x) CONTAINS 'MSSQL') 
RETURN c
```

### **View All GPOs**
```cypher
MATCH (n:GPO) RETURN n
```

### **View All Admin Groups**
```cypher
MATCH (n:Group) WHERE toUpper(n.name) CONTAINS 'ADMIN' RETURN n
```

### **Find AS-REP Roastable Users**
```cypher
MATCH (u:User {dontreqpreauth: true}) RETURN u
```

### **Show High Value Target Groups**
```cypher
MATCH p=(n:User)-[r:MemberOf*1..]->(m:Group {highvalue:true}) RETURN p
```

### **Find Groups with Both Users and Computers**
```cypher
MATCH (c:Computer)-[r:MemberOf*1..]->(groupsWithComps:Group) 
WITH groupsWithComps 
MATCH (u:User)-[r:MemberOf*1..]->(groupsWithComps) 
RETURN DISTINCT(groupsWithComps) AS groupsWithCompsAndUsers
```

### **Find Kerberoastable Users in High Value Groups**
```cypher
MATCH (u:User)-[r:MemberOf*1..]->(g:Group) 
WHERE g.highvalue=true AND u.hasspn=true 
RETURN u
```

### **Find Kerberoastable Users and AdminTo**
```cypher
MATCH (u:User {hasspn:true}) 
OPTIONAL MATCH (u)-[r:AdminTo]->(c:Computer) 
RETURN u
```

### **Find Constrained Delegation Permissions**
```cypher
MATCH (c:Computer) WHERE c.allowedtodelegate IS NOT NULL RETURN c
```

### **Find Domain User Permissions on GPO**
```cypher
MATCH p=(u:User)-[r:AllExtendedRights]->(g:GPO) RETURN p
```

### **Find Unprivileged User Group Add Rights**
```cypher
MATCH (n:User {admincount:False}) 
MATCH p=allShortestPaths((n)-[r:AddMember*1..]->(m:Group)) 
RETURN p
```

### **Find VPN Group Members**
```cypher
MATCH p=(u:User)-[:MemberOf]->(g:Group) 
WHERE toUpper(g.name) CONTAINS 'VPN' 
RETURN p
```

### **Find Active Users That Never Logged On**
```cypher
MATCH (n:User) 
WHERE n.lastlogontimestamp=-1.0 AND n.enabled=TRUE 
RETURN n
```

### **Find Cross-Domain Permissions**
```cypher
MATCH p=(n)-[r]->(m) WHERE NOT n.domain = m.domain RETURN p
```

### **Find User Sessions in a Specific Domain**
```cypher
MATCH p=(m:Computer)-[r:HasSession]->(n:User {domain:'specificDomainValue'}) RETURN p
```
