![Calculating Multi-Hop](./runningMinishift.jpg)

# Introduction
Minikube, which is supposedly a simple way to run Kubernete for Docker, as advertised on Kubernete tutorial page, is very informative. You can read up about it here: Hello [Minikube Kubernetes](https://kubernetes.io/docs/tutorials/hello-minikube) 

However, the documentation there is primarily written based on MacOS. While there are references where you can dig down deeper if you own Windows or different distributions of Linux, the instructions are not so clear. And many instructions like this [minikube/drivers.md at master · kubernetes/minikube · GitHub](https://github.com/kubernetes/minikube/blob/master/docs/drivers.md) is mainly targetted for Debian/Ubuntu. This guide hopefully will make things easier on RHEL/Fedora/CentOS based operating system.

> ASSUMPTION: I will assume that you already had installed Docker on your computer, and you are running RHEL/CentOS/Fedora based workstation.

# Example

The activity sequences are:
u1: AAAA BBB C A
u2: C A BB
u3: CC BBB A C

And we can write as below:

![Calculation from Given Example](./media/diagram1.png)

Calculting the hops result in:

![Data Set You Should Return](./media/diagram2.png)


# Pre-Requisite
1. PostgreSQL
2. 

```sql
-- CREATE Database Table to store the information --
CREATE TEABLE mobility(pk serial primary key, sim_id text, created timestamp, event text, site_id text);

-- INSERT the Sample Data Sets into the mobility table --
INSERT INTO mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 00:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 00:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 01:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 02:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 03:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 04:00', 'sms', 'B');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 05:00', 'sms', 'B');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 06:00', 'sms', 'B');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 07:00', 'sms', 'C');
insert into mobility(sim_id, created, event, site_id) values('0001', timestamp '2018-10-11 08:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0002', timestamp '2018-10-11 00:00', 'sms', 'C');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0002', timestamp '2018-10-11 01:00', 'sms', 'A');
INSERT INTO  mobility(sim_id, created, event, site_id) values('0002', timestamp '2018-10-11 02:00', 'sms', 'B');
```

# Steps
1. Create a directory below where you want to download the files. You will download two files: [minikube](https://github.com/kubernetes/minikube/releases) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl). 
2. Open a Terminal window and run the following command:

```shell
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
> Note that the version of minikube (e.g. minkube-linux-amd64) can change depending your computre's spec.

3. chmod to make it writable.
```shell
chmod +x minikube
```

3. Move the file to /usr/local/bin path. This is to run it as a command.

```shell
mv minikube /usr/local/bin
```

4. Download the kubectl next. Next steps will be pretty much similar as one you did for minikube.

```shell
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
```

> You use the curl command to determine the latest version of kubernete.

5. chmod to make kubectl writable.

```shell
chmod +x kubectl
```

6. Move kubectl to /usr/local/bin path. This is to run it as a command.

```shell
mv kubectl /usr/local/bin
```

7. Now, here is an important part. You need to run minishift start. To do so, you need to have a Hypervisor available. For me, I used KVM2, though Virtualbox is a possibility. This is where this guide reference can help: [Running Kubernetes Locally via Minikube Kubernetes](https://kubernetes.io/docs/setup/minikube). It is necessary to run this as an user instead of root so that the configuration is stored for the user instead of root.

Run the following command.

```shell
minikube start --vm-driver=kvm2
```

It can take quite a while, so wait for it.

8. It will download and start successfully. You use the following command to make sure that minikube started successfully.

```shell
cat ~/.kube/config
```

9. Execute the following command to run the minikube as the context.

```shell
kubectl config use-context minikube
```

10. Then, run the config file command again to check context minikube is there.

```shell
cat ~/.kube/config
```

11. Finally, run the following command to open a browser with Kubernete dashboard.

```
minikube dashboard
```

![Kubernete Dashboard](./dashboard.png)

That is all! After this point, you can follow the rest of tutorials on minikube

 [Running Kubernetes Locally via Minikube Kubernetes](https://kubernetes.io/docs/setup/minikube).