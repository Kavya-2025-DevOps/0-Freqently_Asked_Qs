
# Differences

| Type             | Purpose                                 | Accessible From                     | Typical Use Case                                              |
| ---------------- | --------------------------------------- | ----------------------------------- | ------------------------------------------------------------- |
| **ClusterIP**    | Internal service communication          | Inside the cluster only             | Backend APIs, databases, microservice-to-microservice traffic |
| **NodePort**     | Exposes service on a port of every node | Outside cluster via `<NodeIP>:Port` | Testing, simple external access                               |
| **LoadBalancer** | Creates an external cloud load balancer | Internet or private network         | Production external services                                  |
| **Ingress**      | HTTP/HTTPS routing to multiple services | Internet via a single entry point   | Websites, APIs, path/domain-based routing                     |

##

