# Olympus Microscope Data Collection System

## Overview
A MATLAB-based Graphical User Interface (GUI) designed for medical practitioners to collect and annotate blood cell data at 1000x magnification for malaria detection. The system integrates with microscope coordinate systems to capture high-resolution images of malaria-infected cells.

## System Requirements

### Software Requirements
- MATLAB R2019a or later
- Image Acquisition Toolbox
- Image Processing Toolbox
- IP Webcam App (for Android devices)

### Hardware Requirements
- Olympus Microscope with motorized stage
- X-Y coordinate system (measurement scales: 110-170mm range)
- High-resolution microscope camera OR
- Low-cost microscope setup with IP Webcam
- Computer with USB connectivity

## Features

### 1. Image Acquisition
- **Slide Management**: Enter and manage slide names with unique identifiers
- **Real-time Image Capture**: Live preview from microscope camera
- **Coordinate Recording**: Capture X and Y coordinates of regions of interest
- **Multi-stage Magnification**: Optimized for 1000x magnification imaging

### 2. Malaria Cell Classification
Supports classification of four malaria parasite types:
- **Gametocyte**: Sexual stage parasites
- **Schizont**: Mature stage parasites
- **Trophozoite**: Growing stage parasites
- **Ring**: Early stage parasites
- **None**: Normal cells (no infection)

### 3. Data Organization
Automatic file naming convention:
```
[Prefix]_[Slide_Name]_[DateTime]_[Malaria_Type].extension

Example: Malaria_CM1_17Jun20211104343_1000.tif
```

**Naming Components:**
- `Prefix`: Study identifier (e.g., "Malaria")
- `Slide_Name`: Unique slide identifier (e.g., "CM1")
- `DateTime`: Timestamp (format: DDMmmYYYYHHMMSS)
- `Malaria_Type`: Classification code (1=Gametocyte, 2=Schizont, 3=Trophozoite, 4=Ring, 1000=None)
- `Extension`: .tif or .jpg

## Installation

### Step 1: MATLAB Setup
1. Install MATLAB R2019a or later
2. Ensure Image Acquisition Toolbox is installed
3. Clone or download this repository

### Step 2: Camera Configuration

#### Option A: Built-in Microscope Camera
1. Connect the Olympus microscope camera via USB
2. Configure camera settings in MATLAB:
```matlab
% Check available cameras
imaqhwinfo

% Configure camera adapter
vid = videoinput('adapter_name', device_id);
```

#### Option B: IP Webcam (Low-cost Setup)
1. Install IP Webcam app on Android device
2. Mount smartphone on microscope eyepiece
3. Start IP Webcam server
4. Note the IP address (e.g., http://192.168.1.100:8080)
5. Configure in MATLAB:
```matlab
url = 'http://YOUR_IP:8080/video';
cam = ipcam(url);
```

### Step 3: Coordinate System Calibration
1. Position the microscope stage at origin (0,0)
2. Calibrate X-axis scale (110-170mm range)
3. Calibrate Y-axis scale (0-30mm range)
4. Test coordinate accuracy with known positions

## Usage Guide

### Starting the Application
```matlab
% Navigate to the application directory
cd('path/to/Olympus-Microscope-Data-Collection')

% Run the main GUI
microscope_data_collection_gui
```

### Visual Guide

#### System Overview
![Coordinate System](docs/images/coordinate_system.png)
*Figure 1: X-Y coordinate measurement system on microscope stage*

![GUI Interface](docs/images/gui_interface.png)
*Figure 2: Main data collection interface with malaria type selection*

![File Naming](docs/images/file_naming_convention.png)
*Figure 3: Automatic file naming convention breakdown*

### Workflow

#### 1. Initialize Session
- Enter slide name/identifier in the text field
- Example: "CM1" for Clinical Malaria sample 1

![Step 1: Enter Slide Name](docs/images/gui_interface.png)
> **Figure 2**: Enter the slide name in the text field at the top of the GUI interface

#### 2. Position Microscope
- Manually adjust microscope stage using X-Y controls
- Locate region of interest containing malaria cells
- Note the X and Y coordinate readings on the measurement scales

![Step 2: Read Coordinates](docs/images/coordinate_system.png)
> **Figure 1**: 
> - **Left panel**: X-coordinate scale (horizontal, 110-170mm range)
> - **Right panel**: Y-coordinate scale (vertical, 0-30mm range)
> - Red arrows indicate where to read coordinate values

**Key Points:**
- X-coordinate is read from the horizontal ruler (shown on left side of stage)
- Y-coordinate is read from the vertical ruler (shown on right side of stage)
- Both coordinates are measured in millimeters
- Note the exact position where malaria cells are detected

#### 3. Capture Coordinates
- Record the X coordinate (horizontal scale: 110-170mm)
- Record the Y coordinate (vertical scale: 0-30mm)
- These coordinates mark the exact position for image acquisition

**Coordinate Reading Example:**
```
X = 145mm (from horizontal scale)
Y = 15mm (from vertical scale)
Position: (145, 15)
```

#### 4. Select Malaria Type
- Check the appropriate malaria parasite type:
  - ☐ Gametocyte
  - ☐ Schizont
  - ☐ Trophozoite
  - ☐ Ring
  - ☑ None (for uninfected cells)
- Only one type can be selected at a time

![Step 4: Select Malaria Type](docs/images/gui_interface.png)
> **Figure 2**: Checkbox options for malaria parasite classification
> - Top right section shows malaria type checkboxes
> - Top right displays live microscope view with blood cells
> - Select appropriate parasite stage before capturing

**Classification Guidelines:**
- **Gametocyte**: Sexual stage (banana-shaped)
- **Schizont**: Mature stage (multiple nuclei, 16-32 merozoites)
- **Trophozoite**: Growing stage (larger ring with visible cytoplasm)
- **Ring**: Early stage (ring-shaped with red chromatin dot)
- **None**: No parasites detected (normal blood cells only)

#### 5. Capture and Save
- Click **"Capture Image"** button
- System automatically:
  - Captures high-resolution image
  - Records X-Y coordinates
  - Generates filename with timestamp and classification
  - Saves in designated output folder

![Step 5: Capture Image](docs/images/gui_interface.png)
> **Figure 2**: Click the "Capture Image" button (left side) to save the current view
> - Bottom left shows captured microscope stage position
> - Bottom right shows captured high-magnification cell image

**Captured Data Includes:**
- High-resolution microscope image (1000x magnification)
- X-Y coordinates of the region
- Timestamp of capture
- Selected malaria type classification
- Slide identifier

#### 6. Press Enter
- Click **"Enter"** button to confirm and prepare for next capture
- Clears current selection and readies system for next sample

![Step 6: Complete Entry](docs/images/gui_interface.png)
> **Figure 2**: Click "Enter" button to finalize the entry and reset for next capture

#### 7. Review Saved Files
After capturing, verify your saved files follow the naming convention:

![File Naming Convention](docs/images/file_naming_convention.png)
> **Figure 3**: Automatic filename structure breakdown
> ```
> Malaria_CM1_17Jun20211104343_1000.tif
>    │      │         │           │
>    │      │         │           └─ Malaria Type (1000 = None/Normal)
>    │      │         └─ Date & Time (DDMmmYYYYHHMMSSS)
>    │      └─ Slide Name (from GUI input)
>    └─ Prefix (Study identifier)
> ```

**File Organization:**
- Three files are generated per capture:
  1. Microscope stage view (low magnification, shows position)
  2. High magnification cell image (1000x, shows details)
  3. Metadata log entry (CSV with coordinates and classification)

### Example Session
```
1. Slide Name: CM1
2. Position stage to X=145mm, Y=15mm (see Figure 1)
3. Select: Ring stage parasite (see Figure 2)
4. Click "Capture Image" (see Figure 2)
5. Saved as: Malaria_CM1_17Jun20211104343_4.tif (see Figure 3)
6. Click "Enter" to reset for next capture
7. Move to next region of interest and repeat
```

### Image Setup Instructions

To use this guide, save the images in the following structure:
```
Olympus-Microscope-Data-Collection/
└── docs/
    └── images/
        ├── coordinate_system.png       (Your Image 1)
        ├── gui_interface.png           (Your Image 2)
        └── file_naming_convention.png  (Your Image 3)
```

**Image Sources:**
- `coordinate_system.png`: Microscope X-Y coordinate rulers
- `gui_interface.png`: Main GUI with all controls and live view
- `file_naming_convention.png`: File naming structure example

## Data Collection Protocol

### Coordinate System
- **X-Axis**: Horizontal position (110-170mm range, 60mm travel)
- **Y-Axis**: Vertical position (0-30mm range, 30mm travel)
- **Origin**: Typically set at bottom-left corner of stage
- **Precision**: ±0.1mm coordinate accuracy

### Image Specifications
- **Magnification**: 1000x (oil immersion recommended)
- **Format**: TIFF (lossless) or JPEG
- **Resolution**: Depends on camera (typically 2048x1536 or higher)
- **Color Space**: RGB or Grayscale

### Quality Control
- Ensure proper focus before capture
- Verify adequate illumination
- Check for debris or artifacts
- Validate coordinate readings

## File Structure
```
Olympus-Microscope-Data-Collection/
├── README.md
├── IML_Information.pdf
├── src/
│   ├── microscope_data_collection_gui.m
│   ├── image_capture.m
│   ├── coordinate_manager.m
│   └── file_naming.m
├── data/
│   ├── raw_images/
│   ├── coordinates.csv
│   └── metadata.json
└── docs/
    └── user_manual.pdf
```

## Output Files

### Image Files
- **Location**: `data/raw_images/`
- **Naming**: Automatic with metadata
- **Format**: .tif or .jpg

### Coordinate Log
- **Location**: `data/coordinates.csv`
- **Format**:
```csv
Slide_Name,DateTime,X_Coordinate,Y_Coordinate,Malaria_Type,Filename
CM1,17Jun2021104343,145.2,15.8,Ring,Malaria_CM1_17Jun20211104343_4.tif
```

### Metadata
- **Location**: `data/metadata.json`
- **Contains**: Session info, camera settings, calibration data

## Troubleshooting

### Camera Connection Issues
**Problem**: Camera not detected
- Check USB connection
- Verify camera drivers installed
- Restart MATLAB
- Check camera permissions

**IP Webcam Issues**:
- Verify IP address and port
- Check network connectivity
- Ensure firewall allows connection
- Test URL in web browser first

### Coordinate Reading Errors
**Problem**: Coordinates not accurate
- Recalibrate coordinate system
- Check for mechanical backlash
- Verify scale alignment
- Clean measurement scales

### Image Quality Issues
**Problem**: Blurry or poor quality images
- Adjust microscope focus
- Check illumination intensity
- Clean objective lens
- Verify camera settings
- Use oil immersion for 1000x

### File Saving Errors
**Problem**: Images not saving
- Check write permissions
- Verify sufficient disk space
- Validate output directory path
- Check filename length limits

## Data Analysis Integration

### For Deep Learning (IEEE Research)
The collected data is structured for AI model training:

```matlab
% Load dataset
data = load_microscope_data('data/');

% Prepare for YOLO training
prepare_yolo_dataset(data, 'output_dir');

% Structure matches Ultralytics format
% - images/train/
% - labels/train/
% - data.yaml
```

### Annotation Format
If using for object detection:
```yaml
# Example annotation (YOLO format)
# class x_center y_center width height
0 0.512 0.345 0.123 0.098  # Ring stage
1 0.678 0.567 0.145 0.134  # Trophozoite
```

## Future Enhancements
- [ ] Automated cell detection and counting
- [ ] Real-time AI-based classification
- [ ] Multi-user annotation support
- [ ] Cloud data synchronization
- [ ] Automated stage positioning
- [ ] Integration with laboratory information systems (LIS)

---

**Note**: This system is designed for research purposes. For clinical diagnosis, ensure compliance with local medical device regulations and obtain appropriate approvals.
