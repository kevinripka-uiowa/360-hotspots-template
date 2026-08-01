# 360 Multi-Scene Hotspot Template

Build an interactive 360° experience with multiple panoramic scenes, informational hotspots, media annotations, and scene-to-scene navigation.

This template is designed for documentary, journalism, and interactive storytelling projects. It uses [Pannellum](https://pannellum.org/), an open-source panorama viewer, and can be published as a static website without a database or build process.

## What the template can do

- Display one or more equirectangular 360° panoramas
- Add a separate set of hotspots to each scene
- Open HTML descriptions and media annotations from hotspots
- Move between scenes with teleport hotspots
- Optionally display scene thumbnails for direct navigation
- Set a starting camera position for each scene
- Set a destination camera position for each teleport
- Display images, slideshows, and service-provided audio or video embeds
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

Most projects can be completed by editing these four values:

```js
const P360_SHOW_SCENE_THUMBNAILS = true;
const P360_SHOW_SCENE_TITLE = true;
const P360_START_FULLY_ZOOMED_OUT = true;
const P360_MAX_HFOV = 120;
const P360_FIRST_SCENE = "factory-floor";
const P360_SCENES = { ... };
```

## Configuration overview

```js
const P360_SHOW_SCENE_THUMBNAILS = true;
const P360_SHOW_SCENE_TITLE = true;
const P360_START_FULLY_ZOOMED_OUT = true;
const P360_MAX_HFOV = 120;
const P360_FIRST_SCENE = "factory-floor";

const P360_SCENES = {
  "factory-floor": {
    title: "Factory Floor",
    panorama: "images/test360.jpg",
    thumbnail: "images/test360.jpg",
    thumbnailAlt: "Preview of the factory floor panorama",
    startView: {
      yaw: 0,
      pitch: 0,
      hfov: 120
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
| `P360_SHOW_SCENE_THUMBNAILS` | Boolean | Yes | Set to `true` to show thumbnail navigation or `false` to hide it. |
| `P360_SHOW_SCENE_TITLE` | Boolean | Yes | Set to `true` to show the current scene title over the panorama or `false` to hide it. The title remains available to assistive technology and in the text description. |
| `P360_START_FULLY_ZOOMED_OUT` | Boolean | Yes | When `true`, scenes without their own `startView.hfov` begin at `P360_MAX_HFOV`. |
| `P360_MAX_HFOV` | Number | Yes | The widest horizontal field of view allowed. `120` provides a wide, fully zoomed-out opening view without excessive distortion. |
| `P360_FIRST_SCENE` | String | Yes | The ID of the scene that should load first. It must match a key inside `P360_SCENES`. |
| `P360_SCENES` | Object | Yes | Contains every panorama and its scene-specific configuration. |

## Scene configuration reference

Each scene is stored inside `P360_SCENES` using a unique ID.

```js
"machine-room": {
  title: "Machine Room",
  panorama: "images/scene-02.jpg",
  thumbnail: "images/scene-02.jpg",
  thumbnailAlt: "Preview of the machine room panorama",
  startView: {
    yaw: -35,
    pitch: 0,
    hfov: 120
  },
  hotspots: []
}
```

| Option | Type | Required | Description |
|---|---|---:|---|
| Scene ID | String | Yes | The unique key before the scene object, such as `"machine-room"`. Use lowercase kebab-case without spaces. |
| `title` | String | Recommended | The scene name used in thumbnail navigation, live announcements, and the text description. It also appears over the panorama when `P360_SHOW_SCENE_TITLE` is `true`. |
| `description` | String | Yes | A concise plain-text description of the complete 360° scene. This is used by the accessible text view and live announcements. |
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
| `hfov` | Number | Global setting or `90` | Horizontal field of view. Larger values show more of the panorama. Use `120` for the widest allowed view in this template. |

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

A basic annotation can include a heading and an HTML description:

```js
{
  id: "machine-note",
  type: "annotation",
  label: "Machine detail",
  pitch: 2,
  yaw: 25,
  fallbackIcon: "i",
  title: "Machine Detail",
  description: `
    <p>This machine shaped metal parts used elsewhere in the factory.</p>
    <p><strong>Notice:</strong> the exposed belt drive near the ceiling.</p>
  `
}
```

Use backticks around a multi-line HTML description. Short descriptions can use ordinary quotation marks.

| Option | Type | Required | Description |
|---|---|---:|---|
| `title` | String | No | Heading displayed in the annotation window. |
| `description` | HTML string | No | Annotation content placed below the heading. It can contain trusted HTML such as paragraphs, lists, emphasis, and links. |
| `mediaType` | String | No | Use `"image"`, `"embed"`, or `"slideshow"`. |
| `mediaSrc` | String | Required for image | Path or URL to an annotation image. |
| `mediaAlt` | String | Recommended for image | Alternative text describing an annotation image. |
| `mediaEmbed` | HTML string | Required for embed | The full embed code copied from the audio, video, or other media service. |
| `mediaTitle` | String | Recommended for embed | Accessible name applied to embedded players that do not already include an iframe title or media label. |
| `slides` | Array | Required for slideshow | An array of slideshow objects. |
| `linkUrl` | String | No | URL for an optional link below the annotation. |
| `linkText` | String | Required with `linkUrl` | Visible text for the optional link. |

### HTML descriptions

The `description` property accepts HTML:

```js
description: `
  <p>The east wing was added in <strong>1965</strong>.</p>
  <ul>
    <li>New assembly line</li>
    <li>Expanded loading dock</li>
  </ul>
  <p><a href="https://example.com" target="_blank" rel="noopener">Read the related history</a>.</p>
`
```

Only use HTML that you wrote or obtained from a trusted source. The template deliberately renders this property as HTML, so untrusted code could alter the page or create a security risk. The accessible text view converts the description to plain text.

### Image annotation

```js
{
  id: "historic-photo",
  type: "annotation",
  label: "View a historic photograph",
  pitch: -2,
  yaw: 35,
  title: "Factory Entrance",
  description: "<p>This entrance was used by workers arriving for the morning shift.</p>",
  mediaType: "image",
  mediaSrc: "images/factory-door.jpg",
  mediaAlt: "Historic photograph of workers outside the factory entrance"
}
```

### Hosted audio or video embed

For hosted audio, video, maps, or other media, copy the complete embed code supplied by the service. Do not copy the ordinary page URL. Paste the code inside a JavaScript template literal using backticks.

```js
{
  id: "oral-history",
  type: "annotation",
  label: "Listen to an oral history",
  pitch: 4,
  yaw: -78,
  title: "Oral History Clip",
  description: "<p>A former worker describes the sound of the factory.</p>",
  mediaType: "embed",
  mediaEmbed: `
    <iframe
      title="Oral history audio player"
      width="100%"
      height="166"
      scrolling="no"
      frameborder="no"
      allow="autoplay"
      src="https://w.soundcloud.com/player/?url=YOUR_TRACK_URL">
    </iframe>
  `,
  mediaTitle: "Oral history audio player"
}
```

A YouTube example follows the same pattern:

```js
mediaType: "embed",
mediaEmbed: `
  <iframe
    width="560"
    height="315"
    src="https://www.youtube.com/embed/VIDEO_ID"
    title="Interview with a former factory worker"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
    allowfullscreen>
  </iframe>
`,
mediaTitle: "Interview with a former factory worker"
```

The template inserts the full embed code into the annotation. It also adds lazy loading and a fallback title to embedded iframes when one is missing. Some services block embedding or require privacy, sharing, or publication settings to be changed first.

### Link annotation

```js
{
  id: "related-story",
  type: "annotation",
  label: "Read the related story",
  pitch: 3,
  yaw: -40,
  title: "Related Reporting",
  description: "<p>Read the full article for additional context.</p>",
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
  description: "<p>Use the controls to move through the photographs.</p>",
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
| `text` | String | No | Plain-text caption or explanation for the slide. |

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

Add another property inside `P360_SCENES`. Separate scene objects with commas.

```js
const P360_SCENES = {
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
      hfov: 120
    },
    hotspots: []
  }
};
```

Then add a teleport hotspot in another scene that uses:

```js
targetScene: "loading-dock"
```

A scene does not have to appear in the thumbnail navigation to be reachable by teleport. To create that behavior, leave `P360_SHOW_SCENE_THUMBNAILS` set to `false` and connect scenes only with teleport hotspots.

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

## Scene title

Set:

```js
const P360_SHOW_SCENE_TITLE = true;
```

Set it to `false` to remove the visible title overlay from the panorama. The scene title is still used for thumbnail labels, accessible announcements, and the text description.

The title is positioned to the right of Pannellum’s zoom and fullscreen controls so the controls remain unobstructed.

## Scene thumbnail navigation

Set:

```js
const P360_SHOW_SCENE_THUMBNAILS = true;
```

The navigation appears at the bottom of the viewer and includes every scene in the order listed in `SCENES`.

Set `P360_SHOW_SCENE_THUMBNAILS` to `false` when viewers should move through the experience only by selecting teleport hotspots.

## Accessibility considerations

The template includes keyboard-operable hotspots, keyboard-operable scene buttons, visible focus states, modal close controls, Escape-key support, and a small **Text description** link that opens a nonvisual version of the current scene and its hotspot actions. The outer panorama wrapper is intentionally not placed in the tab order; Pannellum provides its own keyboard focus target. This avoids duplicate focus stops and prevents the page from jumping when users tab into the viewer.

Content creators are still responsible for:

- Writing meaningful hotspot `label` values
- Adding `thumbnailAlt`, `mediaAlt`, and `imageAlt` descriptions
- Providing captions for video and transcripts for audio
- Avoiding hotspot icons that depend only on color
- Making hotspot placement and navigation understandable
- Writing scene descriptions and annotation descriptions that make the same essential information available in the text view

A 360° experience should supplement important reporting rather than make essential information available only through visual exploration.

## Customizing the design

The CSS is included near the top of `index.html`. You can change:

- Hotspot size, shape, and color
- Teleport hotspot styling
- Scene-title placement
- Thumbnail size and navigation placement
- Annotation width and typography
- Mobile layout behavior

Annotation hotspots use the `.p360-hotspot` class. Teleport hotspots also receive the `.p360-is-teleport` class. All template-owned selectors use the `p360-` namespace so the template is less likely to conflict with Tilda or another host page.

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

### Pasting the template into Tilda

The template is namespaced with `p360-` IDs and classes and `P360_` JavaScript constants. Its JavaScript is also enclosed in a private function scope. This reduces conflicts when the code is placed in a Tilda T123 HTML block.

When using the complete template inside Tilda, copy the contents needed by the block and verify that Tilda has not removed external stylesheet or script references. For the most reliable deployment, publish the template separately and embed the finished page with an iframe.

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

### Embedded media does not load

- Copy the service's complete embed code rather than its ordinary page URL.
- Confirm that `mediaType` is `"embed"` and that the code is assigned to `mediaEmbed`.
- Wrap multi-line embed code in backticks, not quotation marks.
- Confirm that the media is public or has embedding enabled.
- Remember that some external websites block iframe embedding.
- Publish or use Live Server when the browser blocks local embedded content.

### The thumbnail image is distorted

Thumbnail images are cropped to a 16:9 frame. Create a separate thumbnail image when the panorama itself does not produce a useful preview.

## Built with

- [Pannellum](https://pannellum.org/)
- HTML
- CSS
- JavaScript

Pannellum is loaded from jsDelivr in `index.html`. The panorama will therefore require an internet connection unless the Pannellum files are downloaded and hosted locally.

## License and media rights

Add a license to the repository if others may reuse the code. The license for the template code does not automatically grant permission to reuse photographs, audio, video, interviews, or other media included in a project.

Credit and document all third-party media according to its license and the standards of the project.
