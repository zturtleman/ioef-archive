# ioef-archive

ioEF (also known as iostvoyHM) is a fork of ioquake3 for running _Star Trek Voyager: Elite Force Holomatch_ (multiplayer). It was created by [Thilo Schulz](https://github.com/thiloschulz).

## Patches

In the [patches](patches) directory you can find the original ioEF source code `.diff` patches that can be applied to [ioquake3 svn repository](https://svn.icculus.org/quake3). The ioquake3 svn revision number is included in the patch file name. The patches are hosted at http://thilo.tjps.eu/efport-progress/patches/.

The patches were mainly known to be at a domain that expired in 2018, [http://thilo.kickchat.com/efport-progress/patches/](https://web.archive.org/web/20180422050624/http://thilo.kickchat.com/efport-progress/patches/) (link to archive.org). This was the primary motivation for creating ioef-archive.

## Branches

In this git repo there are branches with the ioEF patches applied to [ioquake3 git repository](https://github.com/ioquake/ioq3) and some additional changes to fix or simplify compiling. Note that ioquake3 git rewrote the svn history to leave out changes to the website, tags, etc so the EF patch's ioquake3 revision number doesn't directly correspond to the git commit number.

  * efv8-[iorev696](../../commit/08f44d82473c88dad14f1beb23f177f5e364cc20) (missing)
  * efbeta1-[iorev697](../../commit/08f44d82473c88dad14f1beb23f177f5e364cc20) (missing) <!-- Yes, it's the same commit as efv8 due to ioq3 git leaving out the website. -->
  * efbeta2-[iorev716](../../commit/91cfe4a77c8ab20a6d9a23443fbf42426ab8b8e0) (missing)
  * efbeta3-[iorev787](../../commit/d2b5dd1e8a426a4ccd17679e49d14112262e684d) (missing)
  * ef_1.34-[iorev810](../../commit/d42b87ae8707b205d7d8712b0c95fe27c23962da) (missing) (ioEF 1.34 release)
  * ef_1.35-[iorev817](../../commit/2c14f02ee51b195feb025ba7c8f0a65af23b437a) (missing) (ioEF 1.35 release)
  * [ef_1.36-iorev880](../../tree/ef_1.36-iorev880) (ioEF 1.36 release)
  * [ef_1.37-iorev965](../../tree/ef_1.37-iorev965) (ioEF 1.37 release)
  * [ef_rev1245](../../tree/ef_rev1245)
  * [ef_rev1270](../../tree/ef_rev1270)
  * [ef_1.38-iorev1288](../../tree/ef_1.38-iorev1288)
  * [ef_rev1323](../../tree/ef_rev1323)
  * [ef_1.38-iorev1579](../../tree/ef_1.38-iorev1579)
  * [ef_1.38-iorev2058](../../tree/ef_1.38-iorev2058)
  * [ef_1.38-iorev2110](../../tree/ef_1.38-iorev2110)
  * [ef_1.38-iorev2112](../../tree/ef_1.38-iorev2112)
  * [ef_1.38-iorev2123](../../tree/ef_1.38-iorev2123)
  * [ef_1.38-iorev2130](../../tree/ef_1.38-iorev2130) (ioEF 1.38 RC 1 release)
  * [ef_1.38-iorev2231](../../tree/ef_1.38-iorev2231)

## License

ioEF is licensed under [the GNU GPLv2](COPYING.txt) (or at your option, any later version).

## Newer Ports

Thilo Schulz updated it to ioquake3 git in 2016, see [thiloschulz/ioef](https://github.com/thiloschulz/ioef) repo.

For playing Elite Force Holomatch zturtleman would recommend his [Lilium Voyager](https://clover.moe/lilium-voyager) fork (2018).
