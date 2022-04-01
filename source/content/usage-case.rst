使用案例
============

绘制各省地图
-------------

分别绘制中国及河南省的边界。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map

    fig = plt.figure(figsize=(10,10))
    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    draw_map(get_map('中国'), color='k')
    draw_map(get_map('南海'), color='k')
    draw_map(get_map('河南'), color='b')

    plt.show()

.. image:: ../_static/china-line-with-south-sea-and-henan.png

合并省界
---------
将多个省（特区/直辖市）合并起来，我们用很简单的方式来可以绘制一张京津冀的轮廓图。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map

    jingjinji = get_map('北京') + get_map('天津') + get_map('河北')

    fig = plt.figure(figsize=(10,10))
    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    draw_map(jingjinji, color='k')

    plt.show()

.. image:: ../_static/jingjinji.png


绘制青藏高原
------------
cnmaps还内置了青藏高原的边界，可以直接调取使用。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map

    fig = plt.figure(figsize=(10,10))
    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    draw_map(get_map('青藏高原', map_set='geography'), color='k')

    plt.show()

.. image:: ../_static/qingzanggaoyuan.png


根据地图边界裁剪填色等值线
---------------------------
cnmaps可以利用地图边界对等值线图进行裁减，只需要一个 ``clip_contours_by_map`` 函数即可。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_contours_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()
    fig = plt.figure(figsize=(10,10))

    tp = get_map('青藏高原', map_set='geography')

    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    cs = ax.contourf(lons, lats, dem, cmap=plt.cm.terrain)
    clip_contours_by_map(cs, tp)
    draw_map(tp, color='k')

.. image:: ../_static/tp-clip.png


根据边界裁减填色网格图
------------------------
cnmaps也可以对网格图进行裁减，使用 ``clip_pcolormesh_by_map`` 函数即可。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_pcolormesh_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()
    fig = plt.figure(figsize=(10, 10))

    tp = get_map('青藏高原', map_set='geography')

    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    mesh = ax.pcolormesh(lons, lats, dem, cmap=plt.cm.terrain)
    clip_pcolormesh_by_map(mesh, tp)
    draw_map(tp, color='k')
    ax.set_extent(tp.get_extent())

.. image:: ../_static/tp-clip-pcolormesh.png


调整图片边界位置
------------------

我们可以利用 ``get_extent`` 方法获取不同缩放等级的边界，例如下图，我们用12个不同等级的缩放来绘制青藏高原的海拔高度图

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_contours_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()
    fig = plt.figure(figsize=(12,6))
    fig.tight_layout()

    tp = get_map('青藏高原', map_set='geography')

    for i in range(12):
        ax = fig.add_subplot(3,4,i+1, projection=ccrs.PlateCarree())
        cs = ax.contourf(lons, lats, dem, cmap=plt.cm.terrain)
        clip_contours_by_map(cs, tp)
        draw_map(tp, color='k')
        ax.set_extent(tp.get_extent(buffer=i*2))
        plt.title(f'buffer={i*2}')

    plt.show()

.. image:: ../_static/tp-clip-buffer.png


剪切等值线图
--------------
除了填色等值线，非填色的等值线也可以直接用 ``clip_contours_by_map`` 进行剪切。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_contours_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()
    fig = plt.figure(figsize=(18, 9))
    fig.tight_layout()

    tp = get_map('青藏高原', map_set='geography')

    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    cs = ax.contour(lons, lats, dem, cmap=plt.cm.terrain)
    clip_contours_by_map(cs, tp)
    draw_map(tp, color='k')
    ax.set_extent(tp.get_extent(buffer=3))

    plt.show()

.. image:: ../_static/tp-clip-contour.png

对label的裁减
-----------------
cnmaps的clip_clabels_by_map函数可以对超出边界的等值线标签进行裁减。

.. warning:: 由于Cartopy自身的设计缺陷，在0.18.0版本中，Cartopy重写的clabel方法不返回Label Text对象，因此在该版本中 ``clip_clabels_by_map`` 函数无法生效，在0.19.0中修复了这个bug，所以请尽量使用0.19.0及以上版本。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_contours_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()
    fig = plt.figure(figsize=(18, 9))
    fig.tight_layout()

    tp = get_map('青藏高原', map_set='geography')

    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    cs = ax.contour(lons, lats, dem, cmap=plt.cm.terrain)
    clip_contours_by_map(cs, tp)

    cb = ax.clabel(cs, colors='r')
    clip_clabels_by_map(cb, tp)

    draw_map(tp, color='k')
    ax.set_extent(tp.get_extent(buffer=3))

    plt.show()

.. image:: ../_static/tp-clip-contour-labels.png


变换投影
------------
上述的功能在其他投影下也都适用，我们用四种投影来展示一下变换投影的效果。

.. code:: python

    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map, clip_contours_by_map
    from cnmaps.sample import load_dem

    lons, lats, dem = load_dem()

    PROJECTIONS = [
        ('Mercator', ccrs.Mercator(central_longitude=100)),
        ('Mollweide', ccrs.Mollweide(central_longitude=100)),
        ('Orthographic', ccrs.Orthographic(central_longitude=100)),
        ('Robinson', ccrs.Robinson(central_longitude=100))
    ]

    fig = plt.figure(figsize=(16, 12))
    fig.tight_layout()

    china = get_map('中国')

    for i, prj in enumerate(PROJECTIONS):
        ax = fig.add_subplot(2,2,i+1, projection=prj[1])
        cs = ax.contourf(lons, lats, dem, cmap=plt.cm.terrain, transform=ccrs.PlateCarree())
        clip_contours_by_map(cs, china)

        draw_map(china, color='k')
        ax.set_extent(china.get_extent(buffer=3))
        ax.set_global()
        ax.coastlines()
        plt.title(prj[0])

    plt.show()

.. image:: ../_static/china-clip-projections.png