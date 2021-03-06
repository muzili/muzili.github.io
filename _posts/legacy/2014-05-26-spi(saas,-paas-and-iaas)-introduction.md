---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [intro, beginner, jekyll, tutorial]
---
{% include JB/setup %}

啥是"云计算"
============

云计算是一个难以定义并且仍然还在演化的概念，下面是Intel云计算白皮书[^1]给出的云计算的定义。

> 云计算是 IT 消费和交付的演进模式，通过互联网或内部
> 网络以自助服务方式提供，具有灵活、即购即用的业务
> 模式，需要采用高效和可扩展的架构。在云计算架构中，
> 服务和数据存在于共享、动态可扩展的资源池（通常经
> 过虚拟化处理）内。任何经验证的设备都可通过互联网
> 访问这些服务和数据。云计算区别于传统计算的主要属 性包括：
>
> -   计算和存储功能被提取出来，并作为服务提供
> -   服务构建于具备极高扩展能力的基础架构上
> -   服务通过动态、灵活的可配置资源按需提供
> -   服务购买方便，并根据实际使用情况付费
> -   多名用户共享资源（多租户）
> -   任何设备均可通过互联网或内部网络访问服务

云计算的三种服务模式：IaaS，PaaS和SaaS
======================================

{{{more}}} Wikipedia[^2]使用NIST给出了云计算的3中主要服务模式

-   软件即服务（SaaS）：消费者使用应用程序，但并不掌控操作系统、硬件或运作的网络基础架构。是一种服务观念的基础，软件服务供应商，以租赁的概念提供客户服务，而非购买，比较常见的模式是提供一组账号密码。例如：Microsoft
    CRM与Salesforce.com
-   平台即服务（PaaS）：消费者使用主机操作应用程序。消费者掌控运作应用程序的环境（也拥有主机部分掌控权），但并不掌控操作系统、硬件或运作的网络基础架构。平台通常是应用程序基础架构。例如：Google
    App Engine。
-   基础架构即服务（IaaS）：消费者使用“基础计算资源”，如处理能力、存储空间、网络组件或中间件。消费者能掌控操作系统、存储空间、已部署的应用程序及网络组件（如防火墙、负载平衡器等），但并不掌控云基础架构。例如：Amazon
    AWS、Rackspace。

下面的一个图可以概括这3者之间的关系。

![PaaS/IaaS/SaaS的区别](../../../../images/spi/spi_paas_saas_iaas.jpg)

Footnotes
=========

[^1]: <http://www.intel.cn/content/dam/www/public/cn/zh/pdfs/update-I08036-1-Inte-Vision-of-Cloud-Computing-Adoption_cs.pdf>

[^2]: <http://zh.wikipedia.org/wiki/%E9%9B%B2%E7%AB%AF%E9%81%8B%E7%AE%97>\#.E5.8F.82.E8.80.83.E5.BD.A2.E6.80.81

