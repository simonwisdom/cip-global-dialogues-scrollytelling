<script>
  let currentIndex = 0;

  const galleryItems = [
    {
      src: "https://picsum.photos/id/1018/800/600",
      caption: "Where pixels dream of mountains high,<br/>A silent world, a digital sky."
    },
    {
      src: "https://picsum.photos/id/1015/800/600",
      caption: "The river flows, a silver thread,<br/>Through logic gates, its course is sped."
    },
    {
      src: "https://picsum.photos/id/1025/800/600",
      caption: "A lonely peak in circuits born,<br/>Awaiting a synthetic morn."
    },
		{
      src: "https://picsum.photos/id/1039/800/600",
      caption: "The forest breathes a quiet code,<br/>On paths of light, a heavy load."
    }
  ];

  function showNext() {
    currentIndex = (currentIndex + 1) % galleryItems.length;
  }

  function showPrev() {
    currentIndex = (currentIndex - 1 + galleryItems.length) % galleryItems.length;
  }
</script>

<div class="gallery-container">
  <div class="image-wrapper">
    <img src={galleryItems[currentIndex].src} alt="AI-generated romantic art" />
		<div class="caption">{@html galleryItems[currentIndex].caption}</div>
  </div>
  <div class="controls">
    <button on:click={showPrev}>&larr; Previous</button>
    <button on:click={showNext}>Next &rarr;</button>
  </div>
</div>

<style>
  .gallery-container {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
		color: white;
		text-align: center;
		padding: 1rem;
		box-sizing: border-box;
  }
  .image-wrapper {
		position: relative;
    max-width: 100%;
    max-height: calc(100% - 50px);
		margin-bottom: 1rem;
  }
  img {
    max-width: 100%;
    max-height: 100%;
    border-radius: 5px;
		object-fit: contain;
  }
	.caption {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
		background: rgba(0,0,0,0.6);
		padding: 0.5rem;
		font-style: italic;
		font-size: 0.9rem;
	}
  .controls {
    display: flex;
    gap: 1rem;
  }
	button {
		background: white;
		color: #8B0000;
		border: none;
		padding: 0.5rem 1rem;
		border-radius: 3px;
		cursor: pointer;
		font-weight: bold;
	}
	button:hover {
		background: #eee;
	}
</style> 