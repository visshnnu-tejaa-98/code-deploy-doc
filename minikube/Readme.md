# Activity 19

Setup minikube at your local and explore creating namespaces (Go through official documentation)

## Step 1: Installing minikube in windows machine

To install minikube, follwo the below commands or use the official documentation (https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download)

1. To install the installer file

   ```bash
   # To install the installer file
   New-Item -Path 'c:\' -Name 'minikube' -ItemType Directory -Force
   Invoke-WebRequest -OutFile 'c:\minikube\minikube.exe' -Uri 'https://github.com/kubernetes/minikube/releases/latest/download/minikube-windows-amd64.exe' -UseBasicParsing

   ```

2. Add the minikube.exe binary to your PATH. (make sure you run the following command in powershell adminstartor mode )

   ```bash
   $oldPath = [Environment]::GetEnvironmentVariable('Path', [EnvironmentVariableTarget]::Machine)
   if ($oldPath.Split(';') -inotcontains 'C:\minikube'){
   [Environment]::SetEnvironmentVariable('Path', $('{0};C:\minikube' -f $oldPath), [EnvironmentVariableTarget]::Machine)
   }

   ```

3. Staring the minikube

   ![alt text](/images/minikube/cmd2.png)

## Step 2: Installing Kubectl

1. Installing kubectl

   ```bash
   curl.exe -LO "https://dl.k8s.io/release/v1.30.0/bin/windows/amd64/kubectl.exe"
   ```

   ![alt text](/images/minikube/kubectl-install.png)

2. To download the kubectl checksum file

   ```bash
   curl.exe -LO "https://dl.k8s.io/v1.30.0/bin/windows/amd64/kubectl.exe.sha256"
   ```

   ![alt text](/images/minikube/checksum.png)

3. Validate the kubectl binary against the checksum file

   ```bash
    CertUtil -hashfile kubectl.exe SHA256
    type kubectl.exe.sha256
   ```

   ![alt text](/images/minikube/validate-checksum.png)

4. Using powershell to automate verification

   ```bash
    $(Get-FileHash -Algorithm SHA256 .\kubectl.exe).Hash -eq $(Get-Content .\kubectl.exe.sha256)
   ```

   ![alt text](/images/minikube/automate-validation.png)

5. To check the kubectl version

   ```bash
   kubectl version --client
   ```

   ![alt text](/images/minikube/version.png)

## Step 3: Installing Docker

1.  Go to this URl (https://docs.docker.com/desktop/install/windows-install/)

2.  Click on **Docker Desktop for Windows** button

3.  Open the downloded executed file

    ![alt text](/images/minikube/docker-exe-file.png)

4.  Now, the system will reset, post that open powershel and check the docker version

    ```bash
    docker --version
    ```

    ![alt text](/images/minikube/docker-version.png)

5.  Open docker desktop > settings > kubernetes > enable kubernetis > apply > save

    ![alt text](/images/minikube/docker-desktop.png)

6.  Open powershel in adminstrator mode and run the following commands

    ```bash
    kubectl get pods -A
    kubectl get ns

    ```

# Step 4: Exploring namespaces

1. To list all namespaces

   ```bash
   kubectl get ns
   ```

   ![alt text](/images/minikube/ns-list.png)

2. To create a new namespace

   ```bash
   # To create namespace
   kubectl create namespace test-ns

   # TO list namespaces
   kubectl get ns
   ```

   ![alt text](/images/minikube/create-ns.png)

3. To delete a new namespace

   ```bash
   # To list namespaces
   kubectl get ns

   # To delete namespace
   kubectl delete namespace test-ns

   # To list namespace
   kubectl get ns
   ```

   ![alt text](/images/minikube/delete-ns.png)

4. To get a list of all resporces which are namespaced

   ```bash
   kubectl api-resources --namespaced=true
   ```

   ![alt text](/images/minikube/api-ns.png)
