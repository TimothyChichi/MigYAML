vm-template.jinja 
```js
resources:
# [START basic_template]
- name: vm-template
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: zones/us-central1-a/machineTypes/n1-standard-1
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/debian-cloud/global/images/family/debian-8
    networkInterfaces:
    - network: global/networks/default
# [END basic_template]
# [START template_with_variables]
- name: vm-{{ env["deployment"] }}
  type: compute.v1.instance
  properties:
    zone: us-central1-a
    machineType: zones/{{ properties["zone"] }}/machineTypes/n1-standard-1
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/debian-cloud/global/images/family/debian-8
    networkInterfaces:
    - network: global/networks/default
# [END template_with_variables]
```
此 vm-templae 其實就是用 YAML 格式包裝起來，真正丟給 YAML configuration file 的 YAML 檔如下：
```js
# [START use_template_with_variables]
# [START use_basic_template]
imports:
- path: vm-template.jinja

resources:
- name: my-vm
  type: vm-template.jinja
# [END use_basic_template]
  properties:
    zone: us-central1-a
# [END use_template_with_variables]
```
可以看見此 yaml 執行了 import 的動作後，就將 resource 從 vm-template jinja 引入，
其實 gcp template 也可以從單一 YAML 檔直接產生出來。