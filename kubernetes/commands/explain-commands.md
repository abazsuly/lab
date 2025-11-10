# Explain Commands

### List all resources
`kubectl api-resources`

### Explain top level fields of object
`kubectl explain pods`
`kubectl explain ns`

### Deeper investigation of subfields
`kubectl explain pods.spec`
`kubectl explain ns.spec`

### Recursive explain of all available fields
`kubectl explain pods --recursive | less`
`kubectl explain pods.metadata`
