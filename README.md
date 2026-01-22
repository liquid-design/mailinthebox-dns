# mailinthebox-dns

Werking
1.	Voeg records toe in group_vars/<domain>.yml

2.	Run: ansible-playbook -i inventories/production/hosts.yml playbooks/dns_sync.yml

3.	Playbook doet:

	•	Ophalen van bestaande records
	•	Vergelijken met YAML
	•	Alleen PUT doen waar nodig
	•	Declaratieve state matcht YAML
	
We hebben nu een full Ansible role die:
	•	Geen externe plugins gebruikt
	•	Idempotent is
	•	Volledig declaratief werkt met een YAML per domein
	•	Direct uitbreidbaar is naar meerdere record types
	

Dit zijn de commandline voorbeelden dat werkt en deze ansible playbook moet dat reflecteren

Step 1: get token
curl -X POST --user "me@liquid-hosting.be:password\!" "https://mail.liquid-hosting.be/admin/login" -H "X-Auth-Token: 172967"

Step 2: convert to base64
echo -n 'me@liquid-hosting.be:1dacc5a7a56b17dd94b6ba02eda53a2c176cf2e3f500fcbc65bd9048fe63a52a' | base64

Step 3: curl -X GET "https://mail.liquid-hosting.be/admin/mail/users?format=<string>" \
  -H "Authorization: Basic bWVAbGlxdWlkLWhvc3RpbmcuYmU6MWRhY2M1YTdhNTZiMTdkZDk0YjZiYTAyZWRhNTNhMmMxNzZjZjJlM2Y1MDBmY2JjNjViZDkwNDhmZTYzYTUyYQ=="  \
  -H "Content-Type: application/json"
  
  
Step 4: curl -X GET "https://mail.liquid-hosting.be/admin/dns/custom/tmp1.overlaet.com/A" \
-H "Authorization: Basic bWVAbGlxdWlkLWhvc3RpbmcuYmU6MWRhY2M1YTdhNTZiMTdkZDk0YjZiYTAyZWRhNTNhMmMxNzZjZjJlM2Y1MDBmY2JjNjViZDkwNDhmZTYzYTUyYQ==" \
-H "Content-Type: application/json"
[
  {
    "qname": "tmp1.overlaet.com",
    "rtype": "A",
    "sort-order": {
      "created": 0,
      "qname": 0
    },
    "value": "165.232.93.135",
    "zone": "overlaet.com"
  }
]


Step 5: curl -X PUT "https://mail.liquid-hosting.be/admin/dns/custom/tmp10.overlaet.com" \
-H "Authorization: Basic bWVAbGlxdWlkLWhvc3RpbmcuYmU6MWRhY2M1YTdhNTZiMTdkZDk0YjZiYTAyZWRhNTNhMmMxNzZjZjJlM2Y1MDBmY2JjNjViZDkwNDhmZTYzYTUyYQ==" \
-H "Content-Type: application/json" \
-d "165.232.93.140"
updated DNS: overlaet.com
➜  ~ curl -X GET "https://mail.liquid-hosting.be/admin/dns/custom/tmp10.overlaet.com/A" \
-H "Authorization: Basic bWVAbGlxdWlkLWhvc3RpbmcuYmU6MWRhY2M1YTdhNTZiMTdkZDk0YjZiYTAyZWRhNTNhMmMxNzZjZjJlM2Y1MDBmY2JjNjViZDkwNDhmZTYzYTUyYQ==" \
-H "Content-Type: application/json" 
[
  {
    "qname": "tmp10.overlaet.com",
    "rtype": "A",
    "sort-order": {
      "created": 0,
      "qname": 0
    },
    "value": "165.232.93.140",
    "zone": "overlaet.com"
  }
]

Step 6:
curl -X GET "https://mail.liquid-hosting.be/admin/dns/custom/tmp10.overlaet.com/A" \
-H "Authorization: Basic bWVAbGlxdWlkLWhvc3RpbmcuYmU6MWRhY2M1YTdhNTZiMTdkZDk0YjZiYTAyZWRhNTNhMmMxNzZjZjJlM2Y1MDBmY2JjNjViZDkwNDhmZTYzYTUyYQ==" \
-H "Content-Type: application/json"  
[
  {
    "qname": "tmp10.overlaet.com",
    "rtype": "A",
    "sort-order": {
      "created": 0,
      "qname": 0
    },
    "value": "165.232.93.140",
    "zone": "overlaet.com"
  }
]
