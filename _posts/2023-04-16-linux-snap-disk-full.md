---
layout: post
title: snap /dev/loop at 100% utilization -- no free space
date: 2023-04-16
Author: 山猪
tags: [Linux]
comments: true
---

<!-- more -->

Having Snap images which consume 100% of their filesystem is perfectly acceptable. In fact, it's supposed to work that way.

>A snap is a squashfs file carrying content and a bit of metadata that tells the system how to manipulate it. - https://docs.snapcraft.io/snaps/metadata

Because Snap uses SquashFS, which is a compressed read-only filesystem, the filesystem size is always just large enough to contain it's contents. In addition, because the filesystem is read-only, there's no need to allow for any additional storage, as such additional space can never be used anyway.