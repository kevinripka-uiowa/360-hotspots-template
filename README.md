# 360 Multi-Scene Hotspot Template

Build an interactive 360° experience with multiple panoramic scenes, informational hotspots, media annotations, and scene-to-scene navigation.

This template is designed for documentary, journalism, and interactive storytelling projects. It uses [Pannellum](https://pannellum.org/), an open-source panorama viewer, and can be published as a static website without a database or build process.

## What the template can do

- Display one or more equirectangular 360° panoramas
- Add a separate set of hotspots to each scene
- Open text and media annotations from hotspots
- Move between scenes with teleport hotspots
- Optionally display scene thumbnails for direct navigation
- Set a starting camera position for each scene
- Set a destination camera position for each teleport
- Display images, video, audio, embedded media, and slideshows
- Use custom hotspot icons or built-in text symbols
- Support keyboard activation of hotspots and scene buttons
- Print hotspot coordinates to the browser console while editing

## Example uses

- A virtual tour through several rooms or locations
- A documentary scene with interviews and archival material
- A guided exploration of an environment
- A before-and-after comparison using separate scenes
- A spatial story in which viewers choose where to go next

## Prerequisites

You will need:

- Equirectangular 360° images with a 2:1 aspect ratio
- A text editor such as Visual Studio Code
- A local development server such as the Visual Studio Code Live Server extension
- A place to publish static files, such as GitHub Pages

No Pannellum account or API key is required.

## Getting started

### 1. Download or clone the project

Keep the project files together in the same folder.

```text
360-hotspots-template/
├── index.html
├── README.md
├── images/
│   ├── hotspot-default.svg
│   ├── test360.jpg
│   └── scene-02.jpg
└── your-other-media-files
```

You may add folders such as `audio`, `video`, or `slides` to organize your media.

### 2. Open the project in Visual Studio Code

Open the entire project folder rather than opening only `index.html`.

### 3. Run the project with a local server

Using the Live Server extension:

1. Open `index.html`.
2. Right-click inside the editor.
3. Select **Open with Live Server**.

A local server is recommended because browsers may restrict local media or embedded content when an HTML file is opened directly from the computer.

### 4. Edit the configuration

In `index.html`, find the section labeled:

```js
STUDENTS EDIT THIS SECTION
```

Most projects can be completed by editing only these three values:

```js
const SHOW_SCENE_THUMBNAILS = true;
const FIRST_SCENE = "factory-floor";
const SCENES = { ... };
```

## Configuration overview

```js
const SHOW_SCENE_THUMBNAILS = true;
const FIRST_SCENE = "factory-floor";

const SCENES = {
  "factory-floor": {
    title: "Factory Floor",
    panorama: "images/test360.jpg",
    thumbnail: "images/test360.jpg",
    thumbnailAlt: "Preview of the factory floor panorama",
    startView: {
      yaw: 0,
      pitch: 0,
      hfov: 90
    },
    hotspots: [
      // Annotation and teleport hotspots go here.
    ]
  }
};
```

Each property uses JavaScript object syntax. Pay close attention to quotation marks, commas, braces, and brackets.

## Global configuration reference

| Option | Type | Required | Description |
|---|---|---:|---|
| `SHOW_SCENE_THUMBNAILS` | Boolean | Yes | Set to `true` to show thumbnail navigation or `false` to hide it. |
| `FIRST_SCENE` | String | Yes | The ID of the scene that should load first. It must match a key inside `SCENES`. |
| `SCENES` | Object | Yes | Contains every panorama and its scene-specific configuration. |

## Scene configuration reference

Each scene is stored inside `SCENES` using a unique ID.

```js
"machine-room": {
  title: "Machine Room",
  panorama: "images/scene-02.jpg",
  thumbnail: "images/scene-02.jpg",
  thumbnailAlt: "Preview of the machine room panorama",
  startView: {
    yaw: -35,
    pitch: 0,
    hfov: 90
  },
  hotspots: []
}
```

| Option | Type | Required | Description |
|---|---|---:|---|
| Scene ID | String | Yes | The unique key before the scene object, such as `"machine-room"`. Use lowercase kebab-case without spaces. |
| `title` | String | Recommended | The visible scene title and the label shown in thumbnail navigation. |
| `panorama` | String | Yes | Path or URL to the equirectangular 360° image. |
| `thumbnail` | String | No | Image used in scene navigation. If omitted, the panorama image is used. |
| `thumbnailAlt` | String | Recommended | Alternative text describing the thumbnail. |
| `startView` | Object | No | The camera position used when the scene is opened normally. |
| `hotspots` | Array | Yes | The annotation and teleport hotspots belonging to this scene. Use an empty array if the scene has none. |

### Starting-view options

| Option | Type | Default | Description |
|---|---|---:|---|
| `yaw` | Number | `0` | Horizontal camera direction in degrees. |
| `pitch` | Number | `0` | Vertical camera direction in degrees. Positive values look upward; negative values look downward. |
| `hfov` | Number | `90` | Horizontal field of view. Smaller values appear more zoomed in. |

## Hotspot types

The template supports two hotspot types:

- `annotation`: Opens a modal containing text, media, a slideshow, or a link.
- `teleport`: Loads another 360° scene.

Each hotspot belongs to the `hotspots` array of one scene.

## Shared hotspot configuration

These options can be used by both annotation and teleport hotspots.

| Option | Type | Required | Description |
|---|---|---:|---|
| `id` | String | Recommended | A unique identifier for the hotspot. Use lowercase kebab-case. |
| `type` | String | Yes | Use `"annotation"` or `"teleport"`. |
| `label` | String | Recommended | Accessible label and browser tooltip describing the hotspot action. |
| `pitch` | Number | Yes | Vertical hotspot position in the panorama. |
| `yaw` | Number | Yes | Horizontal hotspot position in the panorama. |
| `iconSrc` | String | No | Path to a custom icon image. |
| `iconAlt` | String | No | Alternative text for the icon. Leave empty when the hotspot `label` already communicates its purpose. |
| `iconSize` | Number | No | Hotspot width and height in pixels. |
| `fallbackIcon` | String | No | Text or symbol shown when no custom icon is supplied or the image fails to load. |

## Annotation hotspot configuration

A basic text annotation:

```js
{
  id: "machine-note",
  type: "annotation",
  label: "Machine detail",
  pitch: 2,
  yaw: 25,
  fallbackIcon: "i",
  title: "Machine Detail",
  text: "Explain what the viewer should notice in this part of the scene."
}
```

| Option | Type | Required | Description |
|---|---|---:|---|
| `title` | String | No | Heading displayed in the annotation window. |
| `text` | String | No | Paragraph displayed below the heading. Plain text is supported. |
| `mediaType` | String | No | Use `"image"`, `"video"`, `"audio"`, `"embed"`, or `"slideshow"`. |
| `mediaSrc` | String | Depends | Path or URL to the image, video, audio, or embedded page. Not used for slideshows. |
| `mediaAlt` | String | Recommended for images | Alternative text for an annotation image. |
| `slides` | Array | Required for slideshow | An array of slideshow objects. |
| `linkUrl` | String | No | URL for an optional link below the annotation. |
| `linkText` | String | Required with `linkUrl` | Visible text for the optional link. |

### Image annotation

```js
{
  id: "historic-photo",
  type: "annotation",
  label: "View a historic photograph",
  pitch: -2,
  yaw: 35,
  title: "Factory Entrance",
  text: "This entrance was used by workers arriving for the morning shift.",
  mediaType: "image",
  mediaSrc: "images/factory-door.jpg",
  mediaAlt: "Historic photograph of workers outside the factory entrance"
}
```

### Video annotation

```js
{
  id: "interview-video",
  type: "annotation",
  label: "Watch an interview",
  pitch: 1,
  yaw: 70,
  title: "Worker Interview",
  text: "A former employee describes the work performed in this room.",
  mediaType: "video",
  mediaSrc: "video/interview.mp4"
}
```

Use a web-compatible format such as MP4 with H.264 video when possible.

### Audio annotation

```js
{
  id: "oral-history",
  type: "annotation",
  label: "Listen to an oral history",
  pitch: 4,
  yaw: -78,
  title: "Oral History Clip",
  text: "Listen to a former worker describe the sound of the factory.",
  mediaType: "audio",
  mediaSrc: "audio/oral-history.mp3"
}
```

### Embedded-media annotation

```js
{
  id: "embedded-video",
  type: "annotation",
  label: "Open the embedded video",
  pitch: 0,
  yaw: 100,
  title: "Related Video",
  mediaType: "embed",
  mediaSrc: "https://www.youtube.com/embed/VIDEO_ID"
}
```

Use an embeddable URL rather than the ordinary page URL. Some websites block iframe embedding.

### Link annotation

```js
{
  id: "related-story",
  type: "annotation",
  label: "Read the related story",
  pitch: 3,
  yaw: -40,
  title: "Related Reporting",
  text: "Read the full article for additional context.",
  linkUrl: "https://example.com/story",
  linkText: "Read the full story"
}
```

### Slideshow annotation

```js
{
  id: "archival-slideshow",
  type: "annotation",
  label: "View archival photographs",
  pitch: -1,
  yaw: 145,
  title: "The Factory Through Time",
  text: "Use the controls to move through the photographs.",
  mediaType: "slideshow",
  slides: [
    {
      imageSrc: "slides/factory-01.jpg",
      imageAlt: "The factory exterior in 1942",
      title: "1942",
      text: "The original factory before its eastern addition."
    },
    {
      imageSrc: "slides/factory-02.jpg",
      imageAlt: "The expanded factory exterior in 1965",
      title: "1965",
      text: "The expanded building after production increased."
    }
  ]
}
```

#### Slideshow item reference

| Option | Type | Required | Description |
|---|---|---:|---|
| `imageSrc` | String | Yes | Path or URL to the slide image. |
| `imageAlt` | String | Recommended | Alternative text describing the image. |
| `title` | String | No | Short slide heading. |
| `text` | String | No | Caption or explanation for the slide. |

## Teleport hotspot configuration

Teleport hotspots move the viewer to another scene.

```js
{
  id: "go-to-machine-room",
  type: "teleport",
  label: "Go to the machine room",
  pitch: -5,
  yaw: 112,
  targetScene: "machine-room",
  targetYaw: -35,
  targetPitch: 0,
  targetHfov: 90,
  iconSize: 38,
  fallbackIcon: "➜"
}
```

| Option | Type | Required | Description |
|---|---|---:|---|
| `targetScene` | String | Yes | ID of the scene that should load. It must exactly match a key in `SCENES`. |
| `targetYaw` | Number | No | Horizontal direction shown after arriving. Overrides the destination scene's normal starting yaw. |
| `targetPitch` | Number | No | Vertical direction shown after arriving. Overrides the destination scene's normal starting pitch. |
| `targetHfov` | Number | No | Field of view shown after arriving. Overrides the destination scene's normal starting field of view. |

Destination view settings are useful for maintaining spatial continuity. For example, when a viewer moves through a doorway, the next scene can begin facing away from that doorway.

## Adding another scene

Add another property inside `SCENES`. Separate scene objects with commas.

```js
const SCENES = {
  "factory-floor": {
    // First scene configuration
  },

  "machine-room": {
    // Second scene configuration
  },

  "loading-dock": {
    title: "Loading Dock",
    panorama: "images/loading-dock.jpg",
    thumbnail: "images/loading-dock-thumb.jpg",
    thumbnailAlt: "Preview of the loading dock panorama",
    startView: {
      yaw: 20,
      pitch: 0,
      hfov: 90
    },
    hotspots: []
  }
};
```

Then add a teleport hotspot in another scene that uses:

```js
targetScene: "loading-dock"
```

A scene does not have to appear in the thumbnail navigation to be reachable by teleport. To create that behavior, leave `SHOW_SCENE_THUMBNAILS` set to `false` and connect scenes only with teleport hotspots.

## Finding hotspot coordinates

The template includes a coordinate helper.

1. Open the page with Live Server.
2. Open the browser developer tools.
3. Select the **Console** tab.
4. Rotate the panorama until the desired hotspot location is centered.
5. Click the panorama without clicking an existing hotspot.
6. Copy the printed `pitch` and `yaw` values into the hotspot configuration.

The console message identifies the active scene:

```text
Scene: factory-floor | Hotspot position: pitch: -2.41, yaw: 35.18
```

The coordinate represents the center of the current view, not the exact location of the mouse pointer.

## Working with 360° images

For the panorama to display correctly, use an equirectangular image:

- The image should show a complete 360° horizontal view.
- The width should be twice the height, such as `6000 × 3000` pixels.
- JPEG is usually the most practical format for photographic panoramas.
- Compress large images before publishing so scenes load at a reasonable speed.

Ordinary photographs will appear distorted when used as panoramas.

## Scene thumbnail navigation

Set:

```js
const SHOW_SCENE_THUMBNAILS = true;
```

The navigation appears at the bottom of the viewer and includes every scene in the order listed in `SCENES`.

Set it to `false` when viewers should move through the experience only by selecting teleport hotspots.

## Accessibility considerations

The template includes keyboard-operable hotspots, keyboard-operable scene buttons, visible focus states, modal close controls, and Escape-key support.

Content creators are still responsible for:

- Writing meaningful hotspot `label` values
- Adding `thumbnailAlt`, `mediaAlt`, and `imageAlt` descriptions
- Providing captions or transcripts for audio and video
- Avoiding hotspot icons that depend only on color
- Making hotspot placement and navigation understandable
- Providing an alternative way to access important information when spatial interaction is not accessible to a viewer

A 360° experience should supplement important reporting rather than make essential information available only through visual exploration.

## Customizing the design

The CSS is included near the top of `index.html`. You can change:

- Hotspot size, shape, and color
- Teleport hotspot styling
- Scene-title placement
- Thumbnail size and navigation placement
- Annotation width and typography
- Mobile layout behavior

Annotation hotspots use the `.custom-hotspot` class. Teleport hotspots also receive the `.is-teleport` class.

## Publishing with GitHub Pages

1. Create a new GitHub repository.
2. Upload the project files or push them with Git.
3. Open the repository's **Settings**.
4. Select **Pages**.
5. Under **Build and deployment**, choose **Deploy from a branch**.
6. Select the branch containing the project, usually `main`, and the `/root` folder.
7. Save the settings.

GitHub will provide a public URL after the site is deployed.

Keep all file and folder names lowercase when possible. Web servers treat capitalization literally, so `Scene-02.jpg` and `scene-02.jpg` may be interpreted as different files.

## Embedding the project

After publishing, the project can be embedded in another website with an iframe:

```html
<iframe
  src="https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/"
  title="Interactive 360 tour"
  width="100%"
  height="700"
  style="border: 0;"
  allowfullscreen>
</iframe>
```

Because the viewer fills its available space, the iframe must have an explicit height. Adjust the height for the page and device where it will appear.

## Troubleshooting

### The panorama is black or does not load

- Confirm that the image path matches the actual file location.
- Check capitalization in file and folder names.
- Confirm that the image is an equirectangular panorama.
- Run the project through a local server instead of opening the file directly.
- Check the browser console for missing-file errors.

### A teleport hotspot does nothing

- Confirm that `type` is set to `"teleport"`.
- Confirm that `targetScene` exactly matches a scene ID in `SCENES`.
- Check for a missing comma or brace near the hotspot object.

### A hotspot does not appear

- Confirm that both `pitch` and `yaw` are numbers without quotation marks.
- Rotate through the entire panorama in case the hotspot is behind the starting view.
- Check the browser console for JavaScript errors.

### Media does not load

- Confirm the media path and filename.
- Use browser-compatible media formats.
- Remember that some external websites block iframe embedding.
- Publish or use Live Server when the browser blocks local media access.

### The thumbnail image is distorted

Thumbnail images are cropped to a 16:9 frame. Create a separate thumbnail image when the panorama itself does not produce a useful preview.

## Built with

- [Pannellum](https://pannellum.org/)
- HTML
- CSS
- JavaScript

Pannellum is loaded from jsDelivr in `index.html`. The panorama will therefore require an internet connection unless the Pannellum files are downloaded and hosted locally.


## License and Media Rights

### Template code and documentation

This template was created with substantial assistance from generative AI and is intended to be completely open and free to use.

To the extent that any copyright or related rights exist in the template’s original code, configuration examples, instructions, and documentation, those rights are waived under the **Creative Commons CC0 1.0 Universal Public Domain Dedication**.

You may copy, modify, distribute, publish, teach with, or build upon the template for any purpose, including commercial purposes. Attribution is appreciated but not required.

This dedication applies only to original material created specifically for this template. Third-party libraries, frameworks, and other dependencies remain subject to their own licenses.

### Media rights

The photographs, panoramic images, videos, audio recordings, thumbnails, and other media files included in this repository are provided only as temporary demonstration content.

**The included media is not licensed for reuse.**

Do not copy, redistribute, publish, modify, or use the included media in another project unless you have independently obtained permission from the applicable copyright holder.

Before publishing a project made with this template, replace all demonstration media with material that you created, licensed, or otherwise have permission to use.

The CC0 dedication for the template does not apply to any demonstration media.
