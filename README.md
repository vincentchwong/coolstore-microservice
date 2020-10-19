Red Hat Cool Store Microservice Demo (modified for OCP4.5)
====================================
This is an example demo showing a retail store consisting of several microservices based on [Red Hat OpenShift Application Runtimes](https://www.redhat.com/en/resources/openshift-application-runtimes-datasheet) (Spring Boot, WildFly Swarm, Vert.x, JBoss EAP and Node.js) deployed to [OpenShift](https://access.redhat.com/documentation/en/openshift-container-platform). It demonstrates how to wire up small microservices into a larger application using microservice architectural principals.

Services
--------
There are several individual microservices and infrastructure components that make up this app:

1. Catalog Service - Java application running on [JBoss Web Server (Tomcat)](https://access.redhat.com/products/red-hat-jboss-web-server/) and MongoDB, serves products and prices for retail products
1. Cart Service - Spring Boot application running on JDK which manages shopping cart for each customer
1. Inventory Service - Java EE application running on [JBoss EAP 7](https://access.redhat.com/products/red-hat-jboss-enterprise-application-platform/) and PostgreSQL, serves inventory and availability data for retail products
1. Pricing Service - Business rules application for product pricing on [JBoss BRMS](https://www.redhat.com/en/technologies/jboss-middleware/business-rules)
1. Review Service - WildFly Swarm service running on JDK for writing and displaying reviews for products
1. Rating Service - Vert.x service running on JDK for rating products
1. Coolstore Gateway - Spring Boot + [Camel](http://camel.apache.org) application running on JDK serving as an API gateway to the backend services
1. Web UI - A frontend based on [AngularJS](https://angularjs.org) and [PatternFly](http://patternfly.org) running in a [Node.js](https://access.redhat.com/documentation/en/openshift-container-platform/3.3/paged/using-images/chapter-2-source-to-image-s2i) container.

![Architecture Screenshot](docs/images/arch-diagram.png?raw=true "Architecture Diagram")

![Architecture Screenshot](docs/images/store.png?raw=true "CoolStore Online Shop")

Deploy CoolStore Microservices Application
================
Deploy the CoolStore microservices application using this template `openshift/coolstore-template.yaml`:
```
oc login -u developer
oc new-project coolstore
oc process -f openshift/coolstore-template.yaml | oc create -f -
```

When all pods are deployed, verify all services are functioning:
```
oc rsh $(oc get pods -o name -l app=coolstore-gw)
curl http://catalog:8080/api/products
curl http://inventory:8080/api/availability/329299
curl http://cart:8080/api/cart/FOO
curl http://rating:8080/api/rating/329299
curl http://review:8080/api/review/329299
```
