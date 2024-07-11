# Gif_generator

This project is a Streamlit-based application designed to convert videos into animated GIFs using the `moviepy` library. Users can upload a video, adjust various parameters, preview the frames, and generate downloadable GIFs.

## Features

- Upload video files in `mov` or `mp4` format.
- Adjust scaling of video resolution, playback speed, duration range to export, and frames per second (FPS).
- Preview a frame from the video.
- Display metrics such as width, height, duration, FPS, and total frames of the uploaded video.
- Generate and download animated GIFs based on the selected parameters.

## Modules Used

- **Streamlit (`streamlit`)**: For building the web application interface.
  - Used throughout the application to create the layout, input widgets, and display elements.

- **OS (`os`)**: For file handling operations.
  - Used to read and handle file sizes and paths.

- **Base64 (`base64`)**: For encoding the generated GIF for display in the browser.
  - Used to encode the GIF for preview in the Streamlit app.

- **Tempfile (`tempfile`)**: For handling temporary files.
  - Used to save the uploaded video temporarily for processing.

- **Pillow (`PIL.Image`)**: For image processing.
  - Used to open, resize, and manipulate images extracted from video frames.

- **NumPy (`numpy`)**: For numerical operations and array handling.
  - Used to handle frames of the video as arrays.

- **MoviePy (`moviepy.editor`, `moviepy.video.fx.all`)**: For video processing.
  - Used to read, manipulate, and convert video files into GIFs.

## Code Overview

1. **Session State**: Stores video properties such as width, height, duration, FPS, and total frames.

    ```python
    if 'clip_width' not in st.session_state:
        st.session_state.clip_width = 0
    if 'clip_height' not in st.session_state:
        st.session_state.clip_height = 0
    if 'clip_duration' not in st.session_state:
        st.session_state.clip_duration = 0
    if 'clip_fps' not in st.session_state:
        st.session_state.clip_fps = 0
    if 'clip_total_frames' not in st.session_state:
        st.session_state.clip_total_frames = 0
    ```

2. **Custom Resize Function**: Resizes video frames using Pillow.

    ```python
    def custom_resize(clip, newsize):
        def resize_frame(image):
            pil_image = Image.fromarray(image)
            pil_image = pil_image.resize(newsize, Image.LANCZOS)
            return np.array(pil_image)
        return clip.fl_image(resize_frame)
    ```

3. **File Upload**: Allows users to upload a video file.

    ```python
    uploaded_file = st.file_uploader("Choose a file", type=['mov', 'mp4'])
    ```

4. **Input Parameters**: Users can adjust the scaling of video resolution, playback speed, duration range to export, and FPS.

    ```python
    selected_resolution_scaling = st.slider('Scaling of video resolution', 0.0, 1.0, 0.5)
    selected_speedx = st.slider('Playback speed', 0.1, 10.0, 5.0)
    selected_export_range = st.slider('Duration range to export', 0, int(st.session_state.clip_duration), (0, int(st.session_state.clip_duration)))
    ```

5. **Preview and Metrics Display**: Shows a preview frame from the video and metrics such as width, height, duration, FPS, and total frames.

    ```python
    st.subheader('Preview')
    selected_frame = st.slider('Preview a time frame (s)', 0, int(st.session_state.clip_duration), int(np.median(st.session_state.clip_duration)))
    clip.save_frame('frame.gif', t=selected_frame)
    frame_image = Image.open('frame.gif')
    st.image(frame_image)
    ```

6. **GIF Generation**: Allows users to generate and download an animated GIF based on the selected parameters.

    ```python
    st.subheader('Generate GIF')
    generate_gif = st.button('Generate Animated GIF')
    ```

## How to Run

1. Clone the repository:
    ```bash
    git clone https://github.com/dineshv17/Gif_generator.git
    ```

2. Install the required dependencies:
    ```bash
    pip install streamlit moviepy pillow numpy 
    ```

3. Run the Streamlit app:
    ```bash
    streamlit run streamlit_app.py
    ```

4. Open your web browser and navigate to `http://localhost:8501` to use the application.

## Sample

You can download an example video file from [here](https://github.com/dineshv17/Gif_generator/blob/main/sample.mp4) to test the application.

---

