## Custom Resource Definitions
### Custom Resource Definition
- Defines Custom Resource via yaml file
- Business control surfaces via RBAC authorization (Sales creates request, Finance approves). Only automation can modify the status.
### Custom Resource
- An instantiation of a CRD
- Owns all child objects (Deployments, Services, PVCs) so cleanup of the CR cascades to child objects
### Custom Controller
- Handles CustomResources interacting with external or internal systems.
- Reconciles intent with reality, should be idempotent at all times
- `spec` is automation intent, `status` is observed state 
- Typically does what a human would have to do to manage an application
### CRD vs CR vs Controller
- Custom Resource Definition (CRD) defines the API type
- Custom Resource (CR) is an instance of the CRD yaml
- Controller/Operator is the reconciler that makes reality match to spec and reports status
### Business Usage Examples
1. Interacts with AWS cloud to automatically create a new VPS resource 'CloudNode' while tracking its state, auto-delete hooks once 'CloudNode' is deleted from the cluster triggers auto-deletion of the VPS from the AWS admin control.
2. Creates and maintains an approval to deployment pipeline for customer resources (Customer desires a cluster, Sales needs to approve, Ops needs to approve, then ops team can rent space and build the customers product based on criteria within the CRD)
### Definition Examples

#### CustomResourceDefinition Example
```yml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: flighttickets.flights.com
spec:
  scope: Namespaced
  groups: flights.com
  names:
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets 
    shortNames:
      - ft
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                from: 
                  type: string
                to: 
                  type: string
                number:
                  type: integer
                  minimum: 1
                  maximum: 10
```

#### CustomResource Example
```yml
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Mumbai
  to: London
  number: 2
```
