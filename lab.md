### Minikube explorer
- Tạo 1 cluster bằng minikube
- Check địa chỉ ip của minikube: `minikube ip` 
- Acess vào minikube: `ssh docker@minikube_ip` password: `tcuser`
- Trong node minikube, gõ `docker ps`, ta có thể thấy list các container là các thành phần chính của k8s được cài đặt trong đó. 

### K8s lab:
- Tạo 1 pod nginx: `kubectl run nginx --image=nginx`/
- Sử dụng câu lệnh `kubectl describe pod nginx` để đọc những thông tin của pod. Kéo xuống dưới ta có thể thấy log gồm những quá trình tạo pod./ 
- Ta có thể access vào minikube node để check pod: `docker ps | grep nginx`. Ta có thể thấy 2 container được tạo ra?/
### Deployment: 
- Là tính năng giúp scale up, scale down pods/
- Tạo 1 deployment: `k create deployment deployment-nginx --image=nginx`/
- Ta có thể thấy, 1 pod nginx được tạo ra tự động: `k get pod`/
- Đọc thông tin của deployment này: `k describe deployment deployment-nginx`/
- Các pod được quản lý bởi 1 ReplicaSet (được tạo khi deployment tạo). Tên của pod gồm prefix chính là hash name của replicaset quản lý pod đó./
- Scale up lên 5 pods: `k scale deployment  deployment-nginx --replicas=5`/
- Scale down xuống 3 pods: `k scale deployment  deployment-nginx --replicas=3`/

### Services:
- Tạo 1 services cho deployment-nginx: `k expose deployment deployment-nginx --port=8080 --target-port=80` (port: port service, target-port: port container nginx)/
- Kiểm tra những services đang có: `k get services`. Ta có thể thấy, CLUSTER-IP đã được thay đổi (Không còn là 172...)./
- Access vào node minikube và truy cập vào service vừa tạo:`curl 10.100.97.42:8080`, ta thấy có thể truy cập đến nginx pod./
- Kiểm tra thông tin của service: `k describe services deployment-nginx`, ta có thể thấy các Endpoints là các pod nginx/