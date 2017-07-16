# GDAL2Tiles

Diferentes juegos de comandos que he utilizado para convertir imagenes GeoTif en Tiles, es la forma como cree https://tms.openstreetmap.co

```bash

gdal2tiles --profile=mercator -z 1-8 yourmap.tif outputfolder
gdal2tiles.py --profile=mercator -z 1-19 0012132.tif pos
gdal2tiles.py
gdal2tiles.py --help
gdal2tiles.py --profile=mercator -z 1-19 *.tif pos 
gdal_vrtmerge.py -o merged.vrt 0012132.tif 0012133.tif 0012310.tif 0012311.tif 0012312.tif 0012313.tif 0012330.tif 0012331.tif 0013022.tif 0013023.tif 0013200.tif 0013201.tif 0013202.tif 0013203.tif 0013220.tif 0013221.tif
gdal_merge.py 
gdal_merge.py -o merged.vrt 0012132.tif 0012133.tif 0012310.tif 0012311.tif 0012312.tif 0012313.tif 0012330.tif 0012331.tif 0013022.tif 0013023.tif 0013200.tif 0013201.tif 0013202.tif 0013203.tif 0013220.tif 0013221.tif
ps aux|grep gdal
gdal2tiles.py --profile=mercator -z 1-19 merged.vrt pos 
sudo nano /usr/bin/gdal2tiles.py 
gdal2tiles.py --profile=mercator -z 1-19 merged.vrt pos 
gdal2tiles.py 
gdal2tiles.py --help
ps aux|grep gdal
ps aux|grep gdal
ps aux|grep gdal
pkill -9 gdal
ps aux|grep gdal
pkill -9 gdal
ps aux|grep gdal
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-19 merged.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-8 merged.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-8 merged.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-19 merged.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocoa-Pos-Desastre  --profile=mercator -z 1-19 merged.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocoa-Pos-Desastre  --profile=mercator -z 1-8 merged.vrt pos 
apt-cache search gdalinfo
apt-cache search gdal
gdal2tiles.py --webviewer=all -e  --title=VillaGarzon-Pos-Desastre  --profile=mercator -z 1-19 0013022.tif VillaGarzon
sudo apt-get install gdal-bin
gdalinfo 0013022.tif
gdalinfo 0012312.tif
gdalinfo 0012313.tif
gdalinfo 0012330.tif
gdalinfo 0012331.tif
gdalinfo 0013200.tif
gdalinfo 0013023.tif
gdalinfo *.tif |grep -76.6406250
gdalinfo *.tif |grep "-76.640625"
gdalinfo *.tif 
gdalinfo 0012312.tif
gdalinfo 0012311.tif
gdalinfo 0012310.tif
gdalinfo 0013221.tif
gdalinfo 0013220.tif
gdalinfo 0013203.tif
gdalinfo 0013023.tif
gdalinfo 0013200.tif
gdalinfo 0013201.tif
gdalinfo 0013202.tif
gdalinfo 0013203.tif
gdalinfo 0012310.tif
gdalinfo 0012311.tif
gdalinfo 0012312.tif
gdalinfo 0012313.tif
gdal_merge.py 
gdal_merge.py -o Mocoa.vrt 0012311.tif 0012313.tif 0013200.tif 0013202.tif
gdalinfo Mocoa.vrt 
gdal2tiles.py --webviewer=all -e  --title=VillaGarzon-Pos-Desastre  --profile=mercator -z 1-19 0013022.tif VillaGarzon
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-19 Mocoa.vrt pos 
gdal2tiles.py --webviewer=all -e  --title=Mocos-Pos-Desastre  --profile=mercator -z 1-19 Mocoa.vrt MocoaPos
gdal2tiles.py
gdal2tiles.py --help
gdalinfo ../0013023.tif
git clone https://github.com/commenthol/gdal2tiles-leaflet.git
git clone https://github.com/commenthol/gdal2tiles-leaflet.git
cd gdal2tiles-leaflet/
python gdal2tiles-multiprocess.py 
python gdal2tiles-multiprocess.py  --help
sudo cp gdal2tiles-multiprocess.py /usr/local/bin/
gdal2tiles-multiprocess.py 
gdal2tiles-multiprocess.py 
gdal2tiles-multiprocess.py --webviewer=all -e  --title=VillaGarzon-Pos-Desastre  --profile=mercator -z 1-19 0013022.tif VillaGarzon
gdalinfo 0012132.tif
gdalinfo 
gdalinfo 0012133.tif
gdalinfo 0012310.tif
sudo nano /usr/local/bin/gdal2tiles-multiprocess.py 
gdal_merge.py -o merged.vrt 0012132.tif 0012133.tif 0012310.tif 0012311.tif 0012312.tif 0012313.tif 0012330.tif 0012331.tif 0013022.tif 0013023.tif 0013200.tif 0013201.tif 0013202.tif 0013203.tif 0013220.tif 0013221.tif
gdal2tiles-multiprocess.py --webviewer=all -e  --title=Mocoa-Post-Event-Full  --profile=mercator -z 1-19 merged.vrt MocaoPostEventFull
gdal2tiles-multiprocess.py --webviewer=all -e  --title=Mocoa-Post-Event-Full --profile=mercator -z 1-19 merged.vrt MocaoPostEventFull
gdal2tiles-multiprocess.py --webviewer=all -e  --title=Mocoa-Post-Event-Full --profile=mercator -z 20-22 merged.vrt MocaoPostEventFull
gdal2tiles-multiprocess.py --webviewer=all -e  --title=Mocoa-Post-Event-Full --profile=mercator -z 20-22 merged.vrt MocaoPostEventFull
cat /home/kleper/.bash_history |grep gdal
gdal2tiles-multiprocess.py --webviewer=all -e --title=Carcavas-Pijao --profile=mercator -z 1-22 CarcavasPijaoUMH.tif tms/CarcavasPijao
gdal2tiles-multiprocess.py --webviewer=all -e --title=peque --profile=mercator -z 1-22 Ortofoto-Peque.tiff tms/peque
gdal2tiles-multiprocess.py --webviewer=all -e --title=peque --profile=raster -z 1-22 Ortofoto-Peque.tiff tms/peque
gdal2tiles-multiprocess.py --webviewer=all -e --title=Jardines-del-Corazon --profile=mercator -z 1-22 Jardines_del_corazon.tif tms/Jardines-del-Corazon
gdal2tiles-multiprocess.py --webviewer=all -e --title=centenario --profile=mercator -z 1-22 mosaicocompleto.tif ../tms/centenario
gdal2tiles-multiprocess.py --webviewer=all -e --title=Armenia-Constitcion --profile=mercator -z 1-22 mosaico_recortado_final.tif ../tms/Armenia-constitucion
ps aux|grep gdal
pkill -9 gdal
ps aux|grep gdal
pkill -9 gdal
gdal2tiles-multiprocess.py --webviewer=all -e --title=Armenia-Constitcion --profile=mercator -z 1-22  ../tms/
gdal2tiles-multiprocess.py --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon/
gdalinfo puerto_limon_town1.tif
gdalinfo puerto_limon_island_map_transparent_mosaic_group1.tif 
gdalinfo Jardines_del_corazon.tif
gdalinfo mocoa_1.tif
ps aux |grep gdal
gdalinfo mocoa_1.tif
gdalwarp  -t_srs "EPSG:4326" puerto_limon_town1.tif puerto_limon_town1-out.tif
gdal2tiles-multiprocess.py --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1-out.tif ../tms/puerto-limon/
gdal2tiles --help
gdal2tiles.py --help
gdal2tiles.py --srcnodata=NODATA --resampling=bilinear --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1-out.tif ../tms/puerto-limon/
gdal2tiles.py --srcnodata=256 --resampling=bilinear --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1-out.tif ../tms/puerto-limon/
gdal2tiles.py --srcnodata=256 --resampling=bilinear --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1.tif.ovr ../tms/puerto-limon/
gdal2tiles-multiprocess.py --srcnodata=256 --resampling=bilinear --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon/
gdal2tiles-multiprocess.py --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon/
gdal2tiles-multiprocess.py --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon/
gdal2tiles-multiprocess.py --webviewer=all -e --title=PuertoLimon --profile=mercator -z 1-22 puerto_limon2.tif ../tms/puerto-limon/
gdal2tiles-multiprocess.py --webviewer=all -e --title=La-Pasera-Mocoa --profile=mercator -z 1-22 la\ pasera_transparent_mosaic_group1.tif ../tms/la-pasera-mocoa
gdal2tiles-multiprocess.py --webviewer=all -e --title=La-Pasera-Mocoa --profile=mercator -z 1-18 la\ pasera_transparent_mosaic_group1.tif ../tms/la-pasera-mocoa
gdal2tiles-multiprocess.py --webviewer=all -e --title=La-Pasera-Mocoa --profile=mercator -z 1-22 la\ pasera1.tif ../tms/la-pasera-mocoa
gdal2tiles-multiprocess.py --webviewer=all -e --title=Puerto-Limon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon
gdal2tiles-multiprocess.py --webviewer=all -e --title=Mocoa1 --profile=mercator -z 1-22 mocoa_1.tif ../tms/mocoa1
gdal2tiles-multiprocess.py --webviewer=all -e --title=Puerto-Limon --profile=mercator -z 1-22 puerto_limon_town1.tif ../tms/puerto-limon
gdalinfo la\ pasera1.tif
gdalinfo Jardines_del_corazon.tif
gdalinfo mocoa_1.t
gdalinfo mocoa_1.tif
gdal2tiles-multiprocess.py 
gdal2tiles-multiprocess.py  --help
gdal_translate 
gdal_translate --help
gdal_translate -of vrt -expand rgba puerto_limon_town1-out.tif puerto_limon_town1-out.tif_rgba.vrt
gdal_translate -of vrt -expand rgba puerto_limon_town1-out.tif puerto_limon_town1-out.tif_rgba.vrt
gdalinfo puerto_limon_town1-out.tif 
gdal_translate -of vrt -expand rgba puerto_limon_town1-out.tif puerto_limon_town1-out.tif_rgba.vrt
gdal_translate -of vrt -expand rgb puerto_limon_town1-out.tif puerto_limon_town1-out.tif_rgba.vrt
gdal_translate -of vrt -expand rgb puerto_limon_town1.tif puerto_limon_town1-out.tif_rgba.vrt
gdal_translate --help
gdal_translate -of vrt -expand rgba puerto_limon_town1.tif puerto_limon_town1-out.tif_rgba.vrt
gdalinfo puerto_limon_town1.tif
gdal_translate -of vrt -expand rgba puerto_limon_town1.tif puerto_limon_town1-out.tif_rgba.vrt
gdal_merge.py -o Mocoa2.vrt la\ pasera1.tif puerto_limon2.tif mocoa_1.tif
gdal2tiles-multiprocess.py --webviewer=all -e --title=La-Pesera --profile=mercator -z 1-22 la\ pasera_transparent_mosaic_group1.tif ../tms/pesera
gdal2tiles-multiprocess.py --webviewer=all -e --title=Mocoa-Global-Medic --profile=mercator -z 1-22 Mocoa2.vrt ../tms/mocoa-globalmedic
pkill  -9 gdal
gdal2tiles-multiprocess.py --webviewer=all -e --title=Cubil --profile=mercator -z 1-22 cubil.tif ../tms/cubil

```