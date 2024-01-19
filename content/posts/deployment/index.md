In this post, I will discuss different deployment patterns that you can use to deploy your application into production. The point of these deployment patterns is reduce downtime and provide easy rollbacks incase of a defective deployment.

The effect of these patterns can be observed using application telemetry data.  

## Recreate Deployment

With this deployment pattern, you deploy a new version of your application and delete the existing version of the application. This is the simplest deployment pattern, but it can result in downtime for your application as shown in the following diagram.

![Recreate Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/xaz2odgo91lipr8akqsv.png)

## Ramped

With this deployment pattern, you deploy a new version of your application alongside the existing version of the application. Once the new version is ready, you gradually shift traffic from the existing version to the new version. This is the default deployment strategy in Kubernetes.

![Ramped Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uuv0dddhut62tp4erxa9.png)

## Canary

With this deployment pattern, you deploy a new (canary) version of your application alongside the existing version of the application in production. Once the new version is ready, you shift a small percentage of traffic from the existing version to the new version and keep monitoring the canary version. If the canary version performs well, you continuosly
shift traffic to it until all production traffic is now served by the canary version. If during the traffic shifting, the canary version performs poorly, you stop shifting traffic to it and continue to use the existing version.

![Canary Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3n1sl9681qdtx5co5y5u.png)

## Blue/Green

With this deployment pattern, you deploy a new version (green version) of your application alongside the existing version (blue version) of the application in production. Once the new version is ready to accept traffic, you switch 100% of the production traffic from the existing (blue) version to the new (green) version and then shut down the old version.

![Blue Green Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/02wryehv5zkl1tvxkmfp.png)

## A/B Deployment

With this deployment pattern, you can test a new version of your application by deploying it to a subset of users alongside the existing version of the application.

Usually, traffic is sliced between the versions using request headers/cookies. The load balancer can then direct traffic to different versions of the applications using these request values 

![A/B Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ijuplf9wxsbqxn79n1t6.png)

## Shadow

With this deployment pattern, you can test a new version of your application by deploying it alongside the existing version of the application. However, instead of sending client traffic to the new version, you send a copy of the traffic to the new version. This allows you to test the new version without impacting the existing version.

![Shadow Deployment](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/so7xed95n3mh1r5rzyea.png)

Hopefully, this post has given you a better understanding of different deployment patterns that you can use to deploy your application. 

If I missed any deployment pattern, please do let me know in the comments below.
