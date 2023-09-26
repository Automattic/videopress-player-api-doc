# VideoPress player API
1. [Initialize the api](#initialize-the-api)
	- [Iframe api (typical case)](#iframe-api)
	- [Iframe api with a known id](#known-iframe-id)
	- [JS api (advanced case)](#js-api)

2. [Namespaces](#namespaces)
	- [controls](#controls)
	- [customize](#customize)
	- [info](#info)
	- [status](#status)
## Initialize the api
### Iframe api
How to initialize the iframe api with an unknown frame id. Typical use-case for a WordPress.com / Jetpack rendered post or page.

	<script src="https://v0.wordpress.com/js/videojs/videopress-iframe-api.js"></script>
	<script>
	window.addEventListener( 'message', ( message ) => {

		if ( 'videopress_ready' !== message.data.event ) {
		  return;
		}

		var iframes = document.getElementsByTagName( 'iframe' );
		var unknownIframe = null

		for( var i = 0; i < iframes.length; i++ ) {
			if( event.source === iframes[ i ].contentWindow ) {
				unknownIframe = iframes[ i ]
			}
		}

		if ( ! unknownIframe ) {
			return;
		}

		const api = VideoPressIframeApi(
			unknownIframe,
			() => {
				console.log( 'unknown iframe api loaded!' )

				api.info.onInfoUpdated( async () => {
					console.log( await api.info.guid() )
				} )
			}
		);
	} );
	</script>

### Iframe api with a known id
How to initialize the iframe api with a known iframe id. Typically only used on a custom page you build yourself, where you can create an iframe with a known id.

	<script src="https://v0.wordpress.com/js/videojs/videopress-iframe-api.js"></script>
	<script>
	const iframe = document.getElementById( 'videopress-iframe-id' );
	const api = VideoPressIframeApi(
		iframe,
		() => {
			console.log( 'iframe api loaded!' );
			api.info.onInfoUpdated( async () => {
				console.log( await api.info.guid() )
			} );
		}
	);
	</script>

### JS api
How to initialize the js api with a known player div id. Typically only used on a custom page you build yourself, where you can create a div (to host the video player) with a known id.

	<div id="video-div-id"></div>
	<script src="https://v0.wordpress.com/js/videojs/videopress.js"></script>
	<script>
		const playerEl = document.getElementById( 'video-div-id' );
		// in this example, 8cQHyy5N is the id of the video you want to load.
		const vp = videopress( '8cQHyy5N', playerEl, {} );

		vp.api.info.onInfoUpdated( async () => {
			console.log( 'js api onInfoUpdated')
			console.log( await vp.api.info.guid() )
		} );
	</script>

# Namespaces

## controls

### Functions
#### play
Play the video

##### Example
```javascript
api.controls.play(); // Will start the playback
```
#### pause
Pause the player

##### Example
```javascript
api.controls.pause(); // Will pause the player
```
#### seek
Seek in the video

##### Arguments
- **timeMs** (int) The timestamp to seek to, in milliseconds

##### Return values
- **timeMs** (int) The new player time, in milliseconds

##### Example
```javascript
api.controls.seek( 1000 ); // Will seek the video to the 1000 milliseconds timepoint
```
#### volume
Set the player volume

##### Arguments
- **volume** (int) The volume, as a percentage (0-100)

##### Example
```javascript
api.controls.volume( 10 ); // Will set the volume to 10%
api.controls.volume( 100 ); // Will set the volume to 100%
api.controls.volume( 0 ); // Will set the volume to 0%

```
#### mute
Mute or unmute the player

##### Arguments
- **mute** (bool) Whether to mute or unmute the player

##### Example
```javascript
api.controls.mute( true ); // Will mute the player
api.controls.mute( false ); // Will unmute the player

```
#### fullscreen
Switch the player fullscreen status

##### Arguments
- **fullscreen** (bool) Whether to set the player to fullscreen mode. This event might fail if it doesn't come from (or after) a user action with the player

##### Example
```javascript
// Will set the player to fullscreen (might fail if it doesn't come from a user-generated event)
api.controls.fullscreen( true );
// Will turn off fullscreen mode
api.controls.fullscreen( false );

```
#### setChapters
Sets chapters track for the current video.

##### Arguments
- **chapters** (array) An array containing chapters object defined by { start: int, end: int, title: string }.

##### Return values
- **status** (string) "ok" if the track was set successfully, "<error message>" otherwise.

##### Example
```javascript
api.controls.setChapters( [ { start: 0, end: 1, title: 'Chapter' } ] );
```

## customize

### Functions
#### get
Returns the current player's customization option, if set.

##### Arguments
- **option** (string) The option to get (see arguments of the set method).

##### Return values
- **customized** (boolean) Whether the option is customized or not.
- **value** (mixed) The value of the option, false if not set.

##### Example
```javascript
api.customize.get( 'shareButton' ).then( ( { customized, value } ) => { ... } );
```
#### set
It customizes the player's appearance 
	and behavior by configuring elements such as the control bar, 
	share button, and other related components. 
	It also allows adjustments to various interactions, 
	including the play/pause animation.
	Provide an `options` object to define the desired settings for the player's visual and functional aspects.
	

##### Arguments
- **options** (object) Object with the controls to set:

		- `autoplay` (boolean): Whether the player should start playing as soon as it can.
		- `bigPlayButton` (boolean): Whether the big play button should be visible.
		- `controlBar` (boolean): Whether the control bar should be visible.
		- `hasStarted` (boolean): Whether the player has started playing.
		- `loop` (boolean): Whether the player should start over again when it reaches the end.
		- `muted` (boolean): Whether the player should be muted.
		- `playPauseAnimation` (boolean): Whether the play/pause animation should be visible.
		- `playsinline` (boolean): Whether the player should play inline on supported mobile devices.
		- `poster` (string): The URL of the poster image to show before the video plays.
		- `preload` (string): Whether the player should preload the video.
		- `shareButton` (boolean): Whether the share button should be visible.
		- `showPoster` (boolean): Whether the poster image should be visible.
	

##### Example
```javascript
api.customize.set( { shareButton: false }); // It will hide the share button
```

## info

### Functions
#### duration
Get the video duration in seconds or milliseconds

##### Arguments
- **inMilliseconds** (bool) If true, will return the duration in milliseconds

##### Return values
- **duration** (float) Duration in seconds or milliseconds

##### Example
```javascript
const videoDurationInSeconds = await api.info.duration();
const videoDurationInMilliSeconds = await api.info.duration( true );
```
#### guid
Get the current video GUID

##### Return values
- **guid** (string) The video GUID

##### Example
```javascript
const videoGuid = await api.info.guid();
```
#### title
Get the current video title

##### Return values
- **title** (string) The video title

##### Example
```javascript
const videoTitle = await api.info.title();
```
#### poster
Get the current video poster image URL

##### Return values
- **poster** (string) The video poster image URL

##### Example
```javascript
const videoPosterUrl = await api.info.poster();
```
#### privacy
Get the current video privacy setting (private or public)

##### Return values
- **privacy** (string) The video privacy setting

##### Example
```javascript
const videoPrivacySetting = await api.info.privacy();
const isVideoPrivate = 'private' === videoPrivacySetting;
```
#### getThumbnailSample
Gets an evenly distributed sample of thumbnails. Can only be called after video info has been loaded.

##### Arguments
- **count** (number) The number of thumbnails to retrieve

##### Return values
- **thumbnails** (Array<object>) A series of evenly distributed thumbnails represented by their position in a sprite sheet.
- **size** (object) The width and height of a thumbnail in the sprite sheet.

##### Example
```javascript
const thumbs = await api.info.getThumbnailSample();
```
#### enableWatchedSegments
Enable watched segments tracking

##### Example
```javascript
await api.info.enableWatchedSegments();
```
#### disableWatchedSegments
Disable watched segments tracking

##### Example
```javascript
await api.info.disableWatchedSegments();
```
#### toggleWatchedSegments
Toggle watched segments tracking

##### Example
```javascript
await api.info.toggleWatchedSegments();
```
#### watched
Get the percentage and duration of the video that has been watched

##### Return values
- **percentage** (float) Percentage of the video that has been watched
- **duration** (int) Duration watched, in seconds

##### Example
```javascript
const [ percentage, duration ] = await api.info.watched();
```
#### resetWatched
Reset the watched segments

##### Example
```javascript
await api.info.resetWatched();
```

### Events
#### infoUpdated
Event triggered when media information has changed

##### Example
```javascript
api.status.onInfoUpdated( () => console.log( "Video info updated" ) );
```

## status

### Functions
#### fullscreen
Get the current player fullscreen status

##### Return values
- **isFullscreen** (bool) The current player fullscreen status

##### Example
```javascript
const isPlayerFullscreen = await api.status.fullscreen();
```
#### player
Get the current player status

##### Return values
- **playerStatus** (PlayerStatus) The current player status

##### Example
```javascript
// Player status can either be ready, playing, paused, ended or stalled
const currentPlayerStatus = await api.status.player();
```
#### audio
Get the current player audio status

##### Return values
- **isMuted** (bool) Whether the player is currently muted
- **volume** (number) The current player volume, as a percentage

##### Example
```javascript
const { isMuted, volume } = await api.status.audio();
```
#### playbackTime
Get the cumulative playback time, in seconds

##### Return values
- **playbackTime** (float) The current cumulative playback time, in seconds

##### Example
```javascript
const totalPlaybackTime = await api.status.playbackTime();
```
#### borderColors
Returns the current border colors

##### Return values
- **borderColors** (array) The current border colors

##### Example
```javascript
const borderColors = await player.status.borderColors();
```

### Events
#### fullscreenChanged
Event triggered whenever the current fullscreen status changed

##### Return values
- **isFullscreen** (bool) The current player fullscreen status

##### Example
```javascript
api.status.onFullscreenChanged( ( isFullscreen ) => console.log( 'Current fullscreen status: ' + isFullscreen ) )
```
#### playerStatusChanged
Fired when the player status has changed

##### Return values
- **oldStatus** (PlayerStatus) The old status
- **newStatus** (PlayerStatus) The new status

##### Example
```javascript
api.status.onPlayerStatusChanged(
	( oldStatus, newStatus ) => {
		// Log the old player status
		console.log( 'Old player status was ' + oldStatus );
		// Log the new player status
		console.log( 'New player status is ' + newStatus );
		// Player status can either be ready, playing, paused, ended or stalled
		if ( 'playing' === newStatus ) {
			console.log( 'The player is playing' );
		}
	}
);
```
#### playbackTimeUpdated
Sent whenever the cumulative playback time is updated

##### Return values
- **playbackTime** (float) The current cumulative playback time, in seconds

##### Example
```javascript
api.status.onPlaybackTimeUpdated(
	( newPlaybackTime ) => {
		// If the user watched 10 or more seconds of video, do something.
		if ( newPlaybackTime >= 10 ) {
			doSomething();
		}
	}
);)
```
#### timeUpdate
Triggered when the player time has been updated

##### Return values
- **time** (float) The current player time

##### Example
```javascript
// This will be called every time the playback time gets updated
player.status.onTimeUpdate( ( time ) => {
	console.log( 'New player time is ' + time + 's.' );
} );

```
#### chaptersChapterChange
Triggered when the player chapters track has changed

##### Return values
- **currentChapter** (object) The currently active chapter

##### Example
```javascript
// This will be called every time the currently playing chapter changes.
player.status.onChaptersChapterChange( ( currentChapter ) => {
	console.log( 'New chapters track is ' + JSON.encode( chaptersTrack ) + 's.' );
} );

```
#### chaptersTrackChange
Triggered when the player chapters track has changed

##### Return values
- **chapters** (array) The current chapter track chapters

##### Example
```javascript
// This will be called every time the currently playing chpater changes.
player.status.onChaptersTrackChange( ( chaptersTrack ) => {
	console.log( 'New chapters track is ' + JSON.encode( chaptersTrack ) + 's.' );
} );

```
#### borderColorsChanged
Triggered when the player border colors change

##### Return values
- **borderColors** (array) The current border colors

##### Example
```javascript
// This will be called every time the border colors change.
	player.status.onBorderColorsChanged( ( newBorderColors ) => {
		console.log( 'New border colors are: ' + JSON.encode( newBorderColors ) + '.' );
	} );
	
```
#### error
Triggered when a player error happens.

##### Return values
- **code** (number) The error code
- **message** (string) The error message

##### Example
```javascript
// This will be called every time the player raises an error
player.status.onError( ( code, message ) => {
	console.log( 'Player error ' + code + ': ' + message );
} );
	
```
#### segmentsWatched
Triggered when the user watches a new segment of the video

##### Return values
- **segmentsWatched** (array) The segments of the video that have been watched

##### Example
```javascript
api.status.onSegmentsWatched(
	( segments ) => {
		segments.forEach( ( segment ) => {
			const start = segment[0];
			const end = segment[1];
			// Do something with the segment
		} );
	}
);)
```
#### playbackRateUpdate
Triggered when the playback rate has been updated

##### Return values
- **rate** (float) The current playback rate

##### Example
```javascript
// This will be called every time the playback rate gets updated
player.status.onPlaybackRateUpdate( ( rate ) => {
	console.log( 'New playback rate is ' + rate + '.' );
} );

```

