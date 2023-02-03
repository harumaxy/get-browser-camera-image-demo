<script lang="ts">
  import { onMount } from "svelte";
  const CANVAS_SIZE = 640;

  // initialize html elements
  const media = navigator.mediaDevices.getUserMedia({
    video: {
      width: CANVAS_SIZE,
      height: CANVAS_SIZE,
    },
  });
  let video: HTMLVideoElement;
  media.then((stream) => {
    video.srcObject = stream;
  });
  const canvas = document.createElement("canvas");
  canvas.width = CANVAS_SIZE;
  canvas.height = CANVAS_SIZE;
  const ctx = canvas.getContext("2d");

  const onClick = () => {
    // take snapshot from media
    ctx.drawImage(video, 0, 0, 640, 640);
    const data = canvas.toDataURL("image/png");
    // console.log(data.replace(/^data:image\/(png|jpg);base64,/, ""));

    // download image data
    const link = document.createElement("a");
    link.download = "snapshot.png";
    link.href = data;
    link.click();
  };
</script>

<main>
  <video id="video" width="640" height="640" autoplay bind:this={video} />
  <button on:click={onClick}>Take Snap shot</button>
</main>
