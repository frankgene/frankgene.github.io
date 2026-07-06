This document introduces a practical spatial transcriptomics 3D visualization tool, MAPS-Explore. It covers project management, data management, 2D visualization, slice alignment, 3D visualization, and many other features. Before using this interface, users should first install and start the backend application, and then access it through a frontend browser.

# 1. Install MAPS-Explore
Will be made publicly available after the official publication.

# 2. Start the Backend Application
After installation, enter the application directory. At this point, you have two options:

## (1) Edit Mode
This mode supports all features, including creating and destroying projects, uploading or downloading slice data, running alignment tasks, and project parameter configuration. Start with the following command:

```bash
nohup python manage.py runserver 0.0.0.0:3000 &
```

## (2) Display Mode
This mode only supports project loading and 2D/3D visualization; it does not support any data writing operations, making it suitable for demos and sharing results. Start with the following command:

```bash
nohup python manage.py runserver 0.0.0.0:3000 --read-only &
```

Regardless of the mode, a project log file `nohup.out` will be created in the current folder for subsequent maintenance. The startup commands above by default expose the service on port 3000 of the current device. If your device's port 3000 is occupied, please modify it, and remember to access it using the corresponding port in the browser.

# 3. Frontend Interface Feature Demonstration
## (1) Open MAPS-Explore
If you have MAPS-Explore installed locally, and your local machine has a graphical user interface and any browser, please open the browser and enter the following URL:

```bash
http://127.0.0.1:3000/
```

If you have deployed MAPS-Explore on a remote server, please configure the firewall to open the corresponding ports, and use the following URL:

```bash
http://your_ip:3000/
```

Note: If you have deployed MAPS-Explore in a Docker container, or your MAPS-Explore project needs to be accessed from the external network, you may need to implement access through port forwarding. In this case, you should use the IP and corresponding port of the intermediate server.

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783323239998-1d081ebd-7d9e-47d2-9f7c-12b1b4e81298.png)

## (2) Create a New Project
Click the create project icon in the upper left corner

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783323451493-2c7e44fc-5b44-4696-8460-e2856c1632df.png)

Fill in the project name and description (optional) in the popup window. Note that the project name must be in English and cannot contain spaces or special symbols, otherwise the project will not be loaded correctly!

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783323409635-e572cb76-8eaf-44ed-8f90-6167396aaa8e.png)

When the project name appears in the right-side project tab, the creation is successful.

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783323581624-49b5980f-c5eb-4c57-aab8-14f2f22eaf7f.png)

## (3) Upload Data
Currently, MAPS-Explore only supports H5AD format data uploads. To ensure your data can be correctly recognized and loaded, we recommend using anndata 0.12.11 to process and save slice files; we have not fully tested other versions. Click the Upload button at the lower left corner to select and upload data:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783324017882-bcfef4a3-92d8-4650-aebf-06b2ca9e7cca.png)

The selected H5AD files will be uploaded to the backend in batches. The upload speed depends on the bandwidth:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783324094304-397fea6c-a826-4313-96bc-09077be1f18d.png)

After the upload is complete, you can see the files in the file tree on the left. Click on a file to preview part of its information. Note that MAPS-Explore only displays cell count, gene count, obs information, obsm information, and uns information, and only shows a truncated view of the data. We do not recommend keeping too much information in the H5AD file, as this will reduce upload and loading speed. The data information preview effect is as follows:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783324308757-306eb625-8e8f-4b8a-917a-fbf1a6ca6dba.png)

## (4) Data Processing
The purpose of this step is to improve the efficiency of 2D and 3D visualization. We adopt multiple data acceleration methods and need to convert your H5AD files into compressed binary files to ensure smooth loading and low memory overhead for visualization. First, you need to click on any file to view its information, then click the Process button at the lower left corner. In the popup window, select the obs labels to save, and specify the slice coordinate label (usually in obsm).

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783325240976-8abbef62-3e29-4d16-99c8-117ef94f948d.png)

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783325296353-61cd2e76-173e-4140-b1a4-bb141d8d43ab.png)

We also provide a multi-slice processing feature, allowing you to process all slices uniformly:  
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783325375107-eaf9e043-0c22-44b5-8587-1b012aec2c8f.png)

Click the `Execute` button to start `Process`. The processing may take some time, depending on the slice size you provide and the multi-threading settings (can be set in the `Config` interface). After `Process` ends, the system will navigate to the 2D visualization interface:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783325606741-f650603c-b927-451e-9477-353c0ed37c04.png)

## (5) 2D Visualization
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783326020675-7a01234a-ac8e-439a-b6b2-dc9995c8fb9f.png)

The 2D visualization interface includes multiple visualization features. For the top navigation bar:

+ SLICE dropdown: Used to switch slice files.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327142958-ca94199d-0328-4989-983c-53eab728af7f.png)
+ GENE input box: Used to search the gene list.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327284018-a3573933-92cc-4fb5-b27d-81ef3aae88ab.png)
+ LABEL dropdown: Used to switch cell labels.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327303530-0eb75924-c9e8-46ef-a4fe-778ffeb3692e.png)
+ `Apply` button: When the user changes genes or cell labels, this button needs to be clicked to load data. Data loading will take some time, depending on the data size.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327361170-a7fe6c23-3544-447e-888a-8ee2afcf5e8f.png)
+ dark/light button: Used to switch the background color. By default, it loads a black background, which can be set in the `Config` interface.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327388223-80e22b61-bca1-4af2-a613-3a921c90598c.png)
+ reset button: Used to restore the viewpoint. Users can drag the slice with the right mouse button, and use the mouse wheel to zoom in/out. Clicking this button restores the initial viewpoint and zoom ratio.
+ Window shape button: Used to switch the window shape. The default is rectangle. Click to switch to square; click again to restore the rectangular window.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327424880-54371a06-15ae-4d06-b95f-f4d32a13b487.png)
+ Download button: Click to save the current window as a PNG image. The clarity is affected by the screen size and zoom factor (HD parameter, can be set in the Config interface, default 2X).
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327470399-306f0950-1eaf-48ae-8fed-646e4a3418a9.png)

For the right sidebar:

+ POINT Card: Can change the size and opacity of points.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327515163-4d6c3361-6010-41da-90a2-15aaacc60168.png)
+ LABELS Card: Used to set the color mapping (only for categorical labels; it does not work for numerical type labels such as gene expression values). The dropdown on the right side of the title can switch the color palette (we provide 5 color palettes, which can be set in the Config interface). The three buttons on the right are used for "Export palette", "Select all", and "Deselect all", which are shortcut operations for the multi-select boxes below. The multi-select boxes on the left side of the color entries below can be used to control whether the points of the corresponding label are displayed. Clicking the color rectangle can also switch the mapping color. The `Apply` button below needs to be clicked after modifying colors to confirm the color mapping refresh, and the `Reset` button is used to restore the default color mapping.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327562412-fc30cd86-7aba-4a1b-953e-9366079c3c14.png)
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327606365-1b4e752f-d971-4f5d-b0fe-5410496eb1b4.png)
+ ADVANCED FILTER Card: Used for multi-level label filtering. You can select another label from the dropdown (possibly a more major cell type or region), so as to only display the cell types within that region.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327753729-cc4a0456-0019-4cf9-941c-7055e9bb3eae.png)
+ FLIP Card: Used for mirror flipping of XY axis coordinates.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783327862149-ce9790ef-8b11-4edf-8f4f-cce3ccdb8a81.png)

## (6) Global Slice Alignment
You can launch a global slice alignment task by clicking the Global Align button at the lower left corner, and specify the slices participating in the alignment and their order in the popup window:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783328401710-f9f5cc64-6bb1-44e9-ac06-3441ed9d4323.png)

Note that here I excluded `adata_55` for the purpose of demonstrating the subsequent insertion alignment task. I specified the batch label as the slice order; here you must select the actual slice order, and the elements of the batch column should be of numeric type:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783328479929-86ad8892-33f4-4847-88ae-0d6d57a991a4.png)

The alignment task may take some time to run, because it involves data integration and coordinate alignment calculation:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783329307988-66d205b7-eae4-4f1a-94f7-b7db1c6baf5d.png)

When the task is processed, it will automatically open the 3D visualization interface:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783329405722-31beb487-634a-4be9-a4c0-1af6a6df478b.png)

## (7) 3D Visualization
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783329768556-fd3024bc-0412-48e5-b512-659a524ab7ae.png)

The 3D visualization interface includes richer visualization features. For the top navigation bar:

+ ALIGN dropdown: Used to switch alignment modes. If you have not performed a certain alignment mode but want to access it, it will fall back to a mode that has already been processed.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783329937733-8123aaa8-8682-4c61-8c50-43db3bdef6d7.png)
+ GENE input box: Same as 2D visualization.
+ LABEL dropdown: Same as 2D visualization.
+ Apply button: Same as 2D visualization.
+ dark/light button: Same as 2D visualization.
+ statistics button: Used to expand an additional sidebar to display statistical information.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783330300071-6ff53009-07b4-47ac-9604-64bafa35e655.png)
+ reset button: Used to restore the viewpoint. Users can rotate the space with the left mouse button, drag the slice with the right mouse button, and use the mouse wheel to zoom in/out. Clicking this button restores the initial viewpoint and zoom ratio.
+ Window shape button: Same as 2D visualization.
+ Download button: Same as 2D visualization.

For the right sidebar:

+ POINT Card: Same as 2D visualization.
+ LABELS Card: Same as 2D visualization.
+ ADVANCED FILTER Card: Same as 2D visualization.
+ FILES Card: Used to control whether the points of the corresponding slice are displayed.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783333689608-a07f93f7-bfe2-49e0-9cfb-a23fb15a70fe.png)
+ Shell Panel: The global voxelization panel. Once selected, it will generate a closed, smooth, semi-transparent shell on the outside of the entire point cloud. This shell is entirely computed by the frontend, and is affected by the slice spacing. Users can modify the shell opacity, expansion distance, outline stroke, and lighting rendering effects through the panel. It is worth mentioning that we provide a model download button on the right side, making it convenient to export the shell for further custom rendering in scientific research or other scenarios.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783333734993-ad1c3b2a-9088-4bfe-a3f7-a1d45a91067a.png)
+ Label Shell Panel: The label voxelization panel. Once selected, it will generate a closed, smooth, opaque shell on the outside of the visible point cloud. This shell is entirely computed by the frontend, and is affected by the slice spacing and point density. Users can modify the smoothness and filtering range of the shell through the panel. We also provide a model download button on the right side. Note that due to the low slice density in the demo image, the shell may fail to form a coherent model.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783333641341-d32a1b29-9f62-476d-84a2-202b81e3a565.png)
+ Auto Rotate Panel: The auto-rotation panel. Once selected, you can start rotation, and adjust the rotation speed and direction.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783333857313-6971e8a4-9668-470c-b868-fcd7d9e33d45.png)
+ X Slice Panel: The X-axis thick slab viewfinder. When selected, a cube space appears, and only points inside the cube will be displayed. Users can adjust the position of the cube center by sliding the CENTER slider below, adjust the cube thickness through the THICKNESS slider, and adjust the global x-axis scaling through the X-COMPRESSION slider. In addition, we provide shortcut buttons for x-axis coordinate flip, clockwise rotation, and automatic slide playback on the right side of the title.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783333986853-926d1807-b0ca-49c9-83e6-6fe7965d0b8f.png)
+ Y Slice Panel: The Y-axis thick slab viewfinder, with the same functionality as above.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334412511-007993c5-6f62-4a99-b79f-6377503a30d8.png)
+ Z Slice Panel: The Z-axis thick slab viewfinder, with the same functionality as above. It is worth mentioning that all projects by default scale the Z axis to 0.3, which is to make the slices compact for visualization; this setting can be configured in the Config interface.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334455663-a846effd-57aa-4673-9f51-319b589d81f7.png)

## (8) Reference Slice Alignment
This alignment mode has a different goal from global alignment; it aims to align the two specified slices onto the same plane. You can launch a reference alignment task by clicking the Ref Align button at the lower left corner, and specify the two slices participating in the alignment in the popup window:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334626484-19d69ad9-51f8-484f-84c6-f2382d646f88.png)

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334649504-41d81341-c9a6-4638-9fa0-ff5c2ed19c04.png)

The processing will take some time:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334680480-104a8d90-0aae-4758-81ac-1a5308757159.png)

After processing is complete, the system will navigate directly to the 3D visualization window with some features restricted:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783334759512-34d639be-85c0-477f-9ca9-e568eb9f562a.png)

Specifically, you are restricted to accessing only these two slices, and features such as voxelization, rotation, and thick slab are disabled.

## (9) Insertion Slice Alignment
This alignment mode aims to integrate multiple specified slices that did not participate in global alignment into the already aligned global reference atlas. You can launch an insertion alignment task by clicking the Impute Align button at the lower left corner, and specify the multiple slices participating in the alignment and the insertion positions in the popup window:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335043100-889e251b-da6d-45b3-8a38-31cbddb002fb.png)

You can choose the insertion mode. If you select `Auto-match position`, the system will automatically assign the most similar slice and insert it next to that slice (offset by the target slice's z-axis +0.5). If you select `UNS_atlas_match-First`, the system will load the `uns` information of your specified slice, and extract the target slice index from the `best_atlas_slice_idx` entry under the `atlas_match` field. You can batch-specify insertion positions by setting the contents of this field. The `Auto-match position` logic will also write the corresponding position information into this field. In addition, for a single slice, you can manually specify an insertion position, and the system will insert it next to the target slice (offset by the target slice's z-axis +0.5):

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335467657-983022b8-b101-4255-9951-8b9770cb6b39.png)

Inserting slices will consume time, determined by the slice size and the multi-threading threshold (can be set in the `Config` interface).

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335490175-6b9d3baf-0e62-4c26-bdc9-089142bea0c3.png)

After the insertion is complete, the system will navigate to the 3D visualization interface. You can click the adjustment button at the top to open the list of inserted slices.

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335798045-b2078ed8-a1e3-48a4-a88d-a10bebfdf815.png)

In this list, you can search slices, adjust positions, save positions, batch delete already-inserted slices, and perform other operations.

## (10) Configuration Interface
You can click the Config button at the lower left corner to enter the configuration interface.

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335777034-4d7125a9-9535-4304-bde4-d5a2e2d31c35.png)

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335949191-295f3311-7064-4085-a715-417b36d9f073.png)

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783335977366-1d385c9c-05ee-4d26-ae10-2d0b4008fb7c.png)

The configuration interface can preset the default loading parameters for the project. Each project has an independent configuration file. After the user finishes modifying, they can click save to remember the custom parameter panel. Specifically:

+ Dark / Light BG panel: Used to specify the background color and scatter size/opacity of the default loaded 2D and 3D visualization windows.
+ Palette panel: Used to configure the preset color palette. We provide five color schemes: Palette-1 has 100 preset colors, and Palettes 2-5 each have 20 preset colors. Palette-1 is used as the default preset color scheme. If a certain category label exceeds 100, the jittering palette cycle coloring will be enabled (adding a certain amount of offset while cycling through the color palette to ensure no repeated colors). In addition, we provide a quick palette function. By clicking the sidebar button on the right side of Palette-1, you can open an image viewfinder, where you can upload an image with colors, making it convenient to use the eyedropper tool to quickly configure colors.
+ <!-- This is an image, ocr content: -->
![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783336575675-42ad2833-c184-403f-8db0-61d1b65a6d65.png)
+ Data Process panel: The Threads parameter controls the number of concurrent threads for Process. The Max Cells parameter is used for visualization downsampling, with the purpose of reducing the rendering burden on the frontend, which is helpful for low-performance devices (frontend access devices). The HD Scale parameter is used to control the image saving magnification. The image resolution is determined by the screen resolution × magnification. Typically, a 1080P resolution screen with the default 2× magnification can already achieve relatively clear imaging. Note that due to limitations of the visualization architecture, we cannot provide vector graphics (such as SVG or PDF formats).
+ View panel: The view panel includes multiple visualization parameter presets. XYZ Compress is used to control the scaling ratio of the coordinate axes, where the Z axis is scaled to 0.3 by default, with the purpose of making the slices more compact; Flip XYZ is used to control coordinate mirroring, which is off by default; Rot XYZ is used to control coordinate rotation, with a default of 0; The SHELL panel provides opacity, expansion range, and outline stroke for the global voxelization shell; LABELS SHELL provides three parameters, namely Tolerance (if the specified label has multiple independent clusters, it will select the front n% for calculation), Smooth (the number of iterations of the Gaussian smoothing algorithm; a higher number of iterations results in a smoother surface, but the shell will expand, and there may be a problem of the shell not being closed), and Density (filter out points with low density, ensuring smooth rendering of the main cluster); AUTO ROTATE is used to control the speed and direction of spatial auto-rotation; LIGHTING is used to control the ambient lighting of the shell, including Azimuth (horizontal light angle), Elevation (vertical light angle), Intensity (light intensity), Ambient (ambient light intensity), and Rim (shell reflectivity).

# 4. Demo Data
We provide 6 sets of demo data; users can view them by visiting [http://183.175.59.4/maps/](http://183.175.59.4/maps/).

## (1) example_project
This dataset is used for creating tutorials. It contains a small number of slices, loads quickly, contains a total of 689,539 cells, and the theoretical loading speed for global aligned 3D visualization is about 5 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783337718077-e20419de-fcb1-4095-94d4-281146d8ad76.png)

## (2) demo1_mouse_embro
This data comes from the E11.5 day mouse whole-embryo spatial transcriptomic sequencing by [Spateo](https://www.cell.com/cell/fulltext/S0092-8674(24)01159-0) (doi: 10.1016/j.cell.2024.10.011). It contains 89 slices and 6,918,865 cells. The theoretical loading speed for global aligned 3D visualization is about 20 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783338232137-8dbbcf74-6ae3-4da4-88f4-1b5882329b84.png)

## (3) demo2_mouse_brain
This data comes from the mouse half-brain spatial transcriptomic sequencing on the [Merfish](https://cellxgene.cziscience.com/collections/0cca8620-8dee-45d0-aef5-23f032a5cf09) (doi: 10.1038/s41586-023-06808-9) platform. It contains 129 coronal slices and 3,698,530 cells. The theoretical loading speed for global aligned 3D visualization is about 10 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783338788422-0da5b0aa-ef30-4edf-87a3-2d8ae0d5c69e.png)

## (4) demo3_monkey_brain
This data comes from the marmoset whole-brain cortex spatial transcriptomic sequencing on the [Stereo-seq](https://www.cell.com/cell/fulltext/S0092-8674(23)00679-7) (doi: 10.1016/j.cell.2023.06.009) platform. It contains 119 slices and 30,801,581 cells. The theoretical loading speed for global aligned 3D visualization is about 60 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783339767975-8ef80832-ec28-4a43-ab79-9fb146cfe619.png)

## (5) demo4_monkey_brain
This data comes from the marmoset whole-brain cortex spatial transcriptomic sequencing on the [Stereo-seq](https://www.cell.com/cell/fulltext/S0092-8674(23)00679-7) (doi: 10.1016/j.cell.2023.06.009) platform. It contains 125 slices and 804,294 cells. The theoretical loading speed for global aligned 3D visualization is about 3 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783339916770-d1262885-b3f0-4d85-926b-9873ed413552.png)

## (6) demo5_intergrated_atlas
This data comes from 18 sequencing platforms or datasets of whole-brain or half-brain spatial transcriptomic sequencing. It contains 441 slices and 804,294 cells. The theoretical loading speed for the 3D visualization of global alignment (129 slices, reference atlas) is about 10 seconds, and the theoretical loading speed for the 3D visualization of insertion alignment (441 slices, integrated data) is about 50 seconds:

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783340295212-02da8acd-f970-4910-bba8-c9114b5f5c54.png)

![](https://cdn.nlark.com/yuque/0/2026/png/22773386/1783340241203-760c0959-59a2-4e34-b6fb-ebe3a596e613.png)


