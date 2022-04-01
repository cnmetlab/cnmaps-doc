快速开始
===========
用最简单直接的方式，来绘制你的第一张中国地图。

.. code :: python
    
    import cartopy.crs as ccrs
    import matplotlib.pyplot as plt
    from cnmaps import get_map, draw_map

    fig = plt.figure(figsize=(10,10))
    ax = fig.add_subplot(111, projection=ccrs.PlateCarree())
    draw_map(get_map('中国'), color='k')
    draw_map(get_map('南海'), color='k')

    plt.show()


.. image :: ../_static/china-line-with-south-sea.png