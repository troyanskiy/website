---
id: version-4.0.0-alpha.5-linking-remote-registry
title: Sise asopọ ibi iforukọsilẹ ọlọna jijin kan
original_id: sise asopọ-ibi iforukọsilẹ-ọlọna jijin
---

Verdaccio jẹ aṣoju ikọkọ ati nipa atilẹda [n se asopọ](uplinks.md) ibi iforukọsilẹ ti gbogbogbo.

```yaml
uplinks:
  npmjs:
    url: https://registry.npmjs.org/
```

You can link multiples registries, the following document will drive you throught some helpful configurations.

## Using Asociating Scope

Ọna ara lati wọle si awọn ibi iforukọsilẹ ọlọpọlọpọ nipa lilo `.npmrc` jẹ ilana ẹya ara bi atẹle yii:

    // .npmrc
    registry=htts://registry.npmjs.org
    @mycompany:registry=http://localhost:4873
    

This approache is valid, but comes with several dissadventages:

* O **n ṣiṣẹ pẹlu awọn scope nikan**
* Scope gbọdọ baramu, **ko si igbalaaye fun Awọn ifarahan Yẹpẹrẹ kankan**
* Scope kan **kole sawari lati awọn ibi iforukọsilẹ ọlọpọlọpọ**
* Awọn aami/awọn ọrọ igbaniwọle **gbọdọ wa ni asọye laarin** `.npmrc` atipe jẹ gbigbe wọle sinu ibi ipamọ naa.

Wo apẹẹrẹ kikun kan [nibi](https://stackoverflow.com/questions/54543979/npmrc-multiple-registries-for-the-same-scope/54550940#54550940).

## Sise asopọ Ibi iforukọsilẹ kan

Link a registry is fairly simple, first, define a new section in the `uplinks` section, note the order here is irrelevant.

```yaml
  uplinks:
    private:
      url: https://private.registry.net/npm

    ... [truncated] ...

  'webpack':
    access: $all
    publish: $authenticated
    proxy: private

```

Se afikun abala `aṣoju ikọkọ` lati seto ibi iforukọsilẹ to jẹ yiyan ti o fẹ lati se ni aṣoju ikọkọ.

## Linking Multiples Registry

```yaml
  uplinks:
    server1:
      url: https://server1.registry.net/npm
    server2:
      url: https://server2.registry.net/npm

    ... [truncated] ...

  'webpack':
    access: $all
    publish: $authenticated
    proxy: server1 server2
```

Verdaccio supports multiples registries on the `proxy` field, the request will be resolved with the first in the list, if fails, it will try with the next in the list and so on.

## Ibi iforukọsilẹ Alaisilorila

Having a full Offline Registry is completely possible, if you don't want any connectivity with external remotes you can do the following.

```yaml
<br />auth:
  htpasswd:
    file: ./htpasswd
uplinks:
packages:
  '@my-company/*':
    access: $all
    publish: none
  '@*/*':
    access: $all
    publish: $authenticated
  '**':
    access: $all
    publish: $authenticated
```

Remote all `proxy` fields within each section of `packages`. The registry will became full offline.