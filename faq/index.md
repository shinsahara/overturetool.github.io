---
layout: default
title: Frequently Asked Questions
---

## Frequently Asked Questions

### Q: Why can't I open Overture on MacOS Sierra?

Some Mac OS X users are experiencing issues when they try to open a freshly downloaded copy of Overture. Specifically, the error message that is shown says that >>*"Overture" is damaged and can't be opened. You should move it to the trash*<<:

![Mac OSX Error]({{ site.url }}/images/overture-mac-error.png)

Until we have this issue solved, one possible workaround that enables you to use Overture on your Mac is to download Overture using `wget` as shown below. Note that the following command downloads Overture version 2.4.6. You may want to adjust this command according to the particular version that you are interested in.

```bash
wget https://github.com/overturetool/overture/releases/download/Release/2.4.6/Overture-2.4.6-macosx.cocoa.x86_64.zip
```
Alternatlvly you can either add a GateKeeper rule like:
```bash
spctl --add --label "Overture" /Path/To/Overture
spctl --enable --label "Overture"
```
[Source](https://www.cnet.com/news/how-to-manage-os-x-gatekeeper-from-the-command-line/)

or simply disable GateKeeper:
```bash
# Disable
sudo spctl --master-disable
# Enable
sudo spctl --master-enable
```
[Source](http://osxdaily.com/2016/09/27/allow-apps-from-anywhere-macos-gatekeeper/)