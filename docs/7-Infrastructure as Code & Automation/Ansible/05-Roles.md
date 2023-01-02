---
title: roles
sidebar_position: 5
---

# [Ansible Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)

对于更庞大的项目（如 [kubespray](https://github.com/kubernetes-sigs/kubespray)），单纯的使用 playbooks 仍然显得过于复杂，这时就有必要引入更高层次的抽象——Roles。

Ansible Roles 是一种对 Playbooks 进行分组编排的方法，它通过将 playbook 拆分成更细致的结构，抽取出公共部分，来提高 playbook 配置的重用度。

具体而言，Ansible Roles 要求每个 playbook 都得拆分成如下几个文件夹，并且每个文件夹中必须有一个 `main.yml` 文件（如果该文件夹存在的话），该文件中包含相关内容：

1. tasks 
2. handlers
3. defaults: 变量默认值
4. vars: Role 的其他变量
5. files: 可以通过这个 Role 部署的文件
6. templates: 可以通过这个 ROle 部署的模板
7. meta: Role 的元数据

这些拆分好的文件夹都得分门别类的放在项目根目录的 `roles` 文件夹中。

然后在 `roles` 同级的文件夹（项目根目录）下再新建 playbooks，这些 playbooks 啥也不干，只引用具体的 roles，让它们去干活。

