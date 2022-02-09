# Yulore SoIP : Filtering Tool Related Q&A


## Q: How to check the Filtering tool version?
    
The version of the tel filtering tool can be found in the pdf document suffix or in version.txt

## Q: How to identify if the update-service script execution failed? How can we check the timestamp of last successful execution?
To judge whether the update is successful or not and the last successful update time, you can use the following command - “docker service inspect Yulore-tool-in_tel-filtering”. In the UpdateStatus part of the response information, you can see the update status and the update timestamp like below.
```
updateing：
"UpdateStatus": {
"State": "updating",
"StartedAt": "2022-01-13T12:08:29.728676304Z",
"Message": "update in progress"
}
update completed：
"UpdateStatus": {
"State": "completed",
"StartedAt": "2022-01-13T12:08:29.728676304Z",
"CompletedAt": "2022-01-13T12:08:57.269161135Z",//the time of last successful execution
"Message": "update completed"
}
```

## Q: In case of docker container issue, Re-deployment takes a long time to restore the service. Pls share the procedure to quickly fix the same.

Because the database contains whole numbers we supported, the size is about 500 - 600 MB. If you re-deploy on the whole new environment, it takes time to download the database and import the database. However, if you just update the service with the existing environment and you have more than one service replica, it can be done smoothly and seamlessly with the ability of rolling updates on the docker swarm. During the update time, the service can still work properly. Also, we suggest that you can use deploy-nfs.sh to deploy with NFS if you have multiple service replicas in multiple node so that you can just download once and use it for all service replicas

## Q: How to deploy the container with non-root user?

pls refer: https://docs.docker.com/engine/install/linux-postinstall/


## Q: Do you have High-Availability/Geo-redundant setup of Yulore DB server through update is done?

Our DB service is hosted on AWS with high availability and redundancy, but no Geo-redundant setup currently.

## Q: Average response time for filtering tool query observed is 350 – 400 ms. This must be reduced. Let us know how?
According to our test, one cpu support about 1500qps and response time within one millisecond under one concurrent. we provide some suggests:
 * Please make sure the kernel version is higher than 5.9.12-1.el7.elrepo.x86_64
 * Please test on local machine, which can rule out network effects. 
 * please observe whether the system load is too high during stress test.
 * because filtering tool is CPU-intensive, we recommend that a replica corresponds to a concurrency of less than 5
