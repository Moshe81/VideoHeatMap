in order to create the index .html
I need to run a script ReduceAllOddLinesFromDB.py which will take only half of the reall cordinate in order to save size small as I do not want to use LFS - it is not supported as a web page
                          #this script was ment to take a file that hold cordinate of tracks and to make it much smallrer by taking every second line and not all the lines.
                          
                          input_file = r"D:\Private\Motorcycle\GPX_Scripts\DB\FastApi\ProdMainDB_VideoOnly.csv"
                          output_file = r"D:\Private\Motorcycle\GPX_Scripts\HalfOfProdMainDB_VideoOnly_231124.csv" //update to the date of generation
                          
                          with open(input_file, 'r') as infile, open(output_file, 'w') as outfile:
                              for index, line in enumerate(infile):
                                  if index % 3 == 0:  # Keeps lines with even index (0-based), which are lines 1, 3, 5, ...
                                      outfile.write(line)

Now we need to take the output and to use it for the CreateHeatMap Script 
                        import folium
                        from folium.plugins import HeatMap, LocateControl
                        import pandas as pd
                        import gpxpy
                        
                        gpx_file_path = r"C:\Users\moshira\Documents\Downloads\offroad-4917922528493568.gpx"
                        #df=pd.read_csv(r"D:\Private\Motorcycle\GPX_Scripts\VideoDbPointForTest.csv")
                        #df=pd.read_csv(r"D:\Private\Motorcycle\GPX_Scripts\DB\FastApi\ProdMainDB_VideoOnly.csv")
                        df=pd.read_csv(r"D:\Private\Motorcycle\GPX_Scripts\HalfOfProdMainDB_VideoOnly3.csv")
                        print("The dataframe is:")
                        #specific_columns=df[["lat","lon",'time']]
                        specific_columns=df[["lat","lon"]]
                        print(specific_columns)
                        # create a map object
                        mapObj = folium.Map(
                            [31.66333935897177, 35.22818556773234],
                            tiles='https://israelhiking.osm.org.il/Hebrew/Tiles/{z}/{x}/{y}.png',
                            attr='Israel Hiking Map',
                            zoom_start=8
                        )
                        mapObj.add_child(folium.LatLngPopup())
                        
                        # Add the Israel Hiking Map again for layer control
                        folium.TileLayer(
                            tiles='https://israelhiking.osm.org.il/Hebrew/Tiles/{z}/{x}/{y}.png',
                            attr='Israel Hiking Map',
                            name='Israel Hiking',
                            overlay=False,
                            control=True
                        ).add_to(mapObj)
                        
                        folium.TileLayer(
                            tiles='https://israelhiking.osm.org.il/Hebrew/mtbTiles/{z}/{x}/{y}.png',
                            attr='Israel Biking Map',
                            name='Israel Biking',
                            overlay=False,
                            control=True
                        ).add_to(mapObj)
                        
                        folium.TileLayer(
                            tiles='https://www.google.cn/maps/vt?lyrs=s@189&gl=cn&x={x}&y={y}&z={z}&hl=he',
                            attr='google',
                            name='Google Satellite',
                            overlay=False,
                            control=True
                        ).add_to(mapObj)
                        
                        # Add other tile layers for switching
                        folium.TileLayer('OpenStreetMap', name='OpenStreetMap').add_to(mapObj)
                        
                        # data for heatmap. 
                        # each list item should be in the format [lat, long, value]
                        
                        # Convert DataFrame to list of lists for heatmap
                        data = specific_columns.values.tolist()
                        heatmap_layer = folium.FeatureGroup(name='VideoHeatMap')
                        HeatMap(data).add_to(heatmap_layer)
                        heatmap_layer.add_to(mapObj)
                        
                        # Function to parse GPX file and extract coordinates
                        def parse_gpx(file_path):
                            with open(file_path, 'r') as gpx_file:
                                gpx = gpxpy.parse(gpx_file)
                                points = []
                                for track in gpx.tracks:
                                    for segment in track.segments:
                                        for point in segment.points:
                                            points.append((point.latitude, point.longitude))
                                return points
                        
                        
                        # Parse the GPX file
                        gpx_points = parse_gpx(gpx_file_path)
                        
                        # Add GPX track to the map
                        folium.PolyLine(gpx_points, color='blue', weight=2.5, opacity=1).add_to(mapObj)
                        
                        # Add layer control to toggle between different layers
                        folium.LayerControl().add_to(mapObj)
                        # Add LocateControl to the map
                        LocateControl(auto_start=False).add_to(mapObj)
                        
                        
                        # save the map object as html
                        mapObj.save(r"D:\Private\Motorcycle\GPX_Scripts\MosheVideoHeatMap18112024.html")

Now to rename MosheVideoHeatMap18112024.html to index.html and to locate it in 
Git Vommand
cd C:\Users\moshira\VideoHeatMap\
git checkout new-branch-name

git add index.html (After put the update file)

git commit -m "Add index.html to the repository"

git push origin new-branch-name


Troubleshuting:
IF the script fail make sure the input file is not alleready open in other App

Then to run the script CreatHeatMap.py which will use th output and will generate the HTML file
Copy the content of the HTML to Index.html and to push it to the git and to deploy

I got error "version https://git-lfs.github.com/spec/v1 oid sha256:22dcb671ff3d72ef3fead10d0392d40d6462f027cfeb2f851abedee6c4287c3f size 62074345"
need to handle
The error is because the file is still on LFS - we do not want LFS as it can not be loaded as a webpage even that it can be uploaded to github

Currently the active page is from the new-branch-name branch.

in order to update I need to do 




Then to go to the action TAB and to wait it will get deployed
