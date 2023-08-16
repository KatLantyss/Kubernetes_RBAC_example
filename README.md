# Kubernetes RBAC example

### `curl-pod.yaml` 檔案分為四部分
1. Service Account
2. Pods
3. Role
4. RoleBinding

### 執行指令
```bash
#部署檔案
$ kubectl apply -f curl-pod.yaml

#檢查是否創建成功
$ kubectl get pods 

#進入Pod
$ kubectl exec -it curl-pod -- /bin/sh 

#執行curl並加上TOKEN
$ curl -k -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://kubernetes.default.svc/api/v1/namespaces/default/pods
```

### 執行結果
```json
{
  "kind": "PodList",
  "apiVersion": "v1",
  "metadata": {
    "resourceVersion": "21720"
  },
  "items": [
    {
      "metadata": {
        "name": "curl-pod",
        "namespace": "default",
        "uid": "a613782f-d6f3-47b9-9fee-1dbd351859f4",
        "resourceVersion": "20230",
        "creationTimestamp": "2023-08-16T10:02:16Z",
        "annotations": {
          "cni.projectcalico.org/containerID": "5de328b1eb512bbb913781d732111a2aac4a59fc7066532c03e6eb78b091ece4",
          "cni.projectcalico.org/podIP": "192.168.104.19/32",
          "cni.projectcalico.org/podIPs": "192.168.104.19/32",
          "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"kind\":\"Pod\",\"metadata\":{\"annotations\":{},\"name\":\"curl-pod\",\"namespace\":\"default\"},\"spec\":{\"containers\":[{\"command\":[\"sleep\",\"infinity\"],\"image\":\"curlimages/curl\",\"name\":\"curl-container\"}],\"serviceAccountName\":\"curl-user\"}}\n"
        },
        "managedFields": [
          {
            "manager": "calico",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2023-08-16T10:02:16Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {
              "f:metadata": {
                "f:annotations": {
                  "f:cni.projectcalico.org/containerID": {},
                  "f:cni.projectcalico.org/podIP": {},
                  "f:cni.projectcalico.org/podIPs": {}
                }
              }
            },
            "subresource": "status"
          },
          {
            "manager": "kubectl-client-side-apply",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2023-08-16T10:02:16Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {
              "f:metadata": {
                "f:annotations": {
                  ".": {},
                  "f:kubectl.kubernetes.io/last-applied-configuration": {}
                }
              },
              "f:spec": {
                "f:containers": {
                  "k:{\"name\":\"curl-container\"}": {
                    ".": {},
                    "f:command": {},
                    "f:image": {},
                    "f:imagePullPolicy": {},
                    "f:name": {},
                    "f:resources": {},
                    "f:terminationMessagePath": {},
                    "f:terminationMessagePolicy": {}
                  }
                },
                "f:dnsPolicy": {},
                "f:enableServiceLinks": {},
                "f:restartPolicy": {},
                "f:schedulerName": {},
                "f:securityContext": {},
                "f:serviceAccount": {},
                "f:serviceAccountName": {},
                "f:terminationGracePeriodSeconds": {}
              }
            }
          },
          {
            "manager": "kubelet",
            "operation": "Update",
            "apiVersion": "v1",
            "time": "2023-08-16T10:02:18Z",
            "fieldsType": "FieldsV1",
            "fieldsV1": {
              "f:status": {
                "f:conditions": {
                  "k:{\"type\":\"ContainersReady\"}": {
                    ".": {},
                    "f:lastProbeTime": {},
                    "f:lastTransitionTime": {},
                    "f:status": {},
                    "f:type": {}
                  },
                  "k:{\"type\":\"Initialized\"}": {
                    ".": {},
                    "f:lastProbeTime": {},
                    "f:lastTransitionTime": {},
                    "f:status": {},
                    "f:type": {}
                  },
                  "k:{\"type\":\"Ready\"}": {
                    ".": {},
                    "f:lastProbeTime": {},
                    "f:lastTransitionTime": {},
                    "f:status": {},
                    "f:type": {}
                  }
                },
                "f:containerStatuses": {},
                "f:hostIP": {},
                "f:phase": {},
                "f:podIP": {},
                "f:podIPs": {
                  ".": {},
                  "k:{\"ip\":\"192.168.104.19\"}": {
                    ".": {},
                    "f:ip": {}
                  }
                },
                "f:startTime": {}
              }
            },
            "subresource": "status"
          }
        ]
      },
      "spec": {
        "volumes": [
          {
            "name": "kube-api-access-j4ffv",
            "projected": {
              "sources": [
                {
                  "serviceAccountToken": {
                    "expirationSeconds": 3607,
                    "path": "token"
                  }
                },
                {
                  "configMap": {
                    "name": "kube-root-ca.crt",
                    "items": [
                      {
                        "key": "ca.crt",
                        "path": "ca.crt"
                      }
                    ]
                  }
                },
                {
                  "downwardAPI": {
                    "items": [
                      {
                        "path": "namespace",
                        "fieldRef": {
                          "apiVersion": "v1",
                          "fieldPath": "metadata.namespace"
                        }
                      }
                    ]
                  }
                }
              ],
              "defaultMode": 420
            }
          }
        ],
        "containers": [
          {
            "name": "curl-container",
            "image": "curlimages/curl",
            "command": [
              "sleep",
              "infinity"
            ],
            "resources": {},
            "volumeMounts": [
              {
                "name": "kube-api-access-j4ffv",
                "readOnly": true,
                "mountPath": "/var/run/secrets/kubernetes.io/serviceaccount"
              }
            ],
            "terminationMessagePath": "/dev/termination-log",
            "terminationMessagePolicy": "File",
            "imagePullPolicy": "Always"
          }
        ],
        "restartPolicy": "Always",
        "terminationGracePeriodSeconds": 30,
        "dnsPolicy": "ClusterFirst",
        "serviceAccountName": "curl-user",
        "serviceAccount": "curl-user",
        "nodeName": "asus-proart",
        "securityContext": {},
        "schedulerName": "default-scheduler",
        "tolerations": [
          {
            "key": "node.kubernetes.io/not-ready",
            "operator": "Exists",
            "effect": "NoExecute",
            "tolerationSeconds": 300
          },
          {
            "key": "node.kubernetes.io/unreachable",
            "operator": "Exists",
            "effect": "NoExecute",
            "tolerationSeconds": 300
          }
        ],
        "priority": 0,
        "enableServiceLinks": true,
        "preemptionPolicy": "PreemptLowerPriority"
      },
      "status": {
        "phase": "Running",
        "conditions": [
          {
            "type": "Initialized",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2023-08-16T10:02:16Z"
          },
          {
            "type": "Ready",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2023-08-16T10:02:18Z"
          },
          {
            "type": "ContainersReady",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2023-08-16T10:02:18Z"
          },
          {
            "type": "PodScheduled",
            "status": "True",
            "lastProbeTime": null,
            "lastTransitionTime": "2023-08-16T10:02:16Z"
          }
        ],
        "hostIP": "192.168.1.2",
        "podIP": "192.168.104.19",
        "podIPs": [
          {
            "ip": "192.168.104.19"
          }
        ],
        "startTime": "2023-08-16T10:02:16Z",
        "containerStatuses": [
          {
            "name": "curl-container",
            "state": {
              "running": {
                "startedAt": "2023-08-16T10:02:18Z"
              }
            },
            "lastState": {},
            "ready": true,
            "restartCount": 0,
            "image": "docker.io/curlimages/curl:latest",
            "imageID": "docker.io/curlimages/curl@sha256:bb0843a1307b1aa73f65f24379d11dde881c16db62ba50810de0c64d48e740ed",
            "containerID": "containerd://616e37ab8ffd1f9720ccea49695c01d27e1566362107fb59c0b938da28481c3f",
            "started": true
          }
        ],
        "qosClass": "BestEffort"
      }
    }
  ]

```