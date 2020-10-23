# 1603470422 puppet-agent-catalog-cache
On a Ubuntu VM the puppet agent will store the cached catalog in the following location:

```
/opt/puppetlabs/puppet/cache/client_data/catalog
```

The following command can be execited to retrieve this catalog, cache it and apply the configuration inside of the catalog.

```bash
puppet agent --test
```

The catalog by default will be a `.json` file and will be located in the directory mentioned above. The contents of this catalog will look like:

```json
{
  "tags": [
    "settings",
    "9e50cf884c7d",
    "node"
  ],
  "name": "9e50cf884c7d",
  "version": 1603470271,
  "code_id": null,
  "catalog_uuid": "7e2b0d71-4751-46f2-92f8-4ffc1ef0e400",
  "catalog_format": 1,
  "environment": "production",
  "resources": [
    {
      "type": "Stage",
      "title": "main",
      "tags": [
        "stage"
      ],
      "exported": false
    },
    {
      "type": "Class",
      "title": "Settings",
      "tags": [
        "class",
        "settings"
      ],
      "exported": false
    },
    {
      "type": "Class",
      "title": "Main",
      "tags": [
        "class"
      ],
      "exported": false,
      "parameters": {
        "name": "main"
      }
    },
    {
      "type": "File",
      "title": "AgentTestfileALL.txt",
      "tags": [
        "file",
        "agenttestfileall.txt",
        "class"
      ],
      "file": "/etc/puppetlabs/code/environments/production/manifests/site.pp",
      "line": 2,
      "exported": false,
      "parameters": {
        "path": "/tmp/AgentTestfileALL.txt",
        "ensure": "present",
        "mode": "0777",
        "content": "Hello ALL agents \n"
      }
    },
    {
      "type": "Node",
      "title": "9e50cf884c7d",
      "tags": [
        "node",
        "9e50cf884c7d",
        "class"
      ],
      "exported": false
    },
    {
      "type": "File",
      "title": "AgentTestfile1.txt",
      "tags": [
        "file",
        "agenttestfile1.txt",
        "node",
        "9e50cf884c7d",
        "class"
      ],
      "file": "/etc/puppetlabs/code/environments/production/manifests/site.pp",
      "line": 11,
      "exported": false,
      "parameters": {
        "path": "/tmp/AgentTestfile1.txt",
        "ensure": "present",
        "mode": "0777",
        "content": "Hello agent 9e50cf884c7d \n"
      }
    }
  ],
  "edges": [
    {
      "source": "Stage[main]",
      "target": "Class[Settings]"
    },
    {
      "source": "Stage[main]",
      "target": "Class[Main]"
    },
    {
      "source": "Class[Main]",
      "target": "File[AgentTestfileALL.txt]"
    },
    {
      "source": "Class[Main]",
      "target": "Node[9e50cf884c7d]"
    },
    {
      "source": "Node[9e50cf884c7d]",
      "target": "File[AgentTestfile1.txt]"
    }
  ],
  "classes": [
    "settings",
    "9e50cf884c7d"
  ]
}
```



## Links
