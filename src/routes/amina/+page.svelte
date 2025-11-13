<script>
	import { onMount } from 'svelte';

	let animationId = null;
	let resizeHandler = null;

	onMount(() => {
		// Load Three.js if not already loaded
		if (typeof window.THREE === 'undefined') {
			const script = document.createElement('script');
			script.src = 'https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js';
			script.onload = initScene;
			script.onerror = () => {
				document.getElementById('loading').style.display = 'none';
				const errorDiv = document.getElementById('error');
				errorDiv.style.display = 'block';
				errorDiv.textContent = 'Failed to load Three.js. Please refresh the page.';
			};
			document.head.appendChild(script);
		} else {
			initScene();
		}

		// Add touch event listeners
		document.addEventListener('touchmove', preventTouchMove, { passive: false });
		document.addEventListener('gesturestart', preventGesture);

		// Cleanup function
		return () => {
			if (animationId) {
				cancelAnimationFrame(animationId);
				animationId = null;
			}
			if (resizeHandler) {
				window.removeEventListener('resize', resizeHandler);
			}
			document.removeEventListener('touchmove', preventTouchMove);
			document.removeEventListener('gesturestart', preventGesture);
		};
	});

	function initScene() {
		const THREE = window.THREE;

		// Mobile detection and settings
		const isMobile = window.innerWidth < 768;
		const roseCount = isMobile ? 20 : 50;
		const textSizeMultiplier = Math.min(1, window.innerWidth / 800);

		// Scene setup
		const scene = new THREE.Scene();
		const camera = new THREE.PerspectiveCamera(
			75,
			window.innerWidth / window.innerHeight,
			0.1,
			1000
		);
		const renderer = new THREE.WebGLRenderer({ antialias: !isMobile, alpha: true });

		renderer.setSize(window.innerWidth, window.innerHeight);
		renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
		renderer.setClearColor(0x000000, 0);
		document.getElementById('canvas-container').appendChild(renderer.domElement);

		camera.position.z = isMobile ? 20 : 15;

		// Lighting
		const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
		scene.add(ambientLight);

		const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
		directionalLight.position.set(10, 10, 10);
		scene.add(directionalLight);

		const pointLight = new THREE.PointLight(0xff69b4, 1, 100);
		pointLight.position.set(0, 5, 10);
		scene.add(pointLight);

		// Global variables
		let textMesh = null;
		let roses = [];
		let floatOffset = 0;

		// Create rose emoji texture
		function createRoseTexture() {
			const canvas = document.createElement('canvas');
			canvas.width = 64;
			canvas.height = 64;
			const ctx = canvas.getContext('2d');

			// Draw emoji
			ctx.font = '48px Arial';
			ctx.textAlign = 'center';
			ctx.textBaseline = 'middle';
			ctx.fillText('🌹', 32, 32);

			const texture = new THREE.Texture(canvas);
			texture.needsUpdate = true;
			return texture;
		}

		const roseTexture = createRoseTexture();

		// Create falling roses
		function createRoses() {
			const spriteMaterial = new THREE.SpriteMaterial({
				map: roseTexture,
				transparent: true,
				opacity: 0.9
			});

			for (let i = 0; i < roseCount; i++) {
				const sprite = new THREE.Sprite(spriteMaterial.clone());
				sprite.scale.set(0.5, 0.5, 0.5);

				// Random starting positions
				sprite.position.x = (Math.random() - 0.5) * 30;
				sprite.position.y = Math.random() * 20 + 10;
				sprite.position.z = (Math.random() - 0.5) * 20;

				// Random falling speeds
				sprite.userData = {
					speedY: Math.random() * 0.03 + 0.02,
					speedX: (Math.random() - 0.5) * 0.01,
					rotation: Math.random() * Math.PI * 2,
					rotationSpeed: (Math.random() - 0.5) * 0.02
				};

				scene.add(sprite);
				roses.push(sprite);
			}
		}

		// Update falling roses
		function updateRoses() {
			roses.forEach((rose) => {
				rose.position.y -= rose.userData.speedY;
				rose.position.x += rose.userData.speedX;
				rose.userData.rotation += rose.userData.rotationSpeed;

				// Reset to top when fallen below view
				if (rose.position.y < -10) {
					rose.position.y = 20;
					rose.position.x = (Math.random() - 0.5) * 30;
					rose.position.z = (Math.random() - 0.5) * 20;
				}
			});
		}

		// Load font and create 3D text
		const loader = new THREE.FontLoader();

		loader.load(
			'https://threejs.org/examples/fonts/helvetiker_bold.typeface.json',
			function (font) {
				document.getElementById('loading').style.display = 'none';

				const textGeometry = new THREE.TextGeometry('I love Amina\nso much', {
					font: font,
					size: 1.5 * textSizeMultiplier,
					height: 0.5,
					curveSegments: 12,
					bevelEnabled: true,
					bevelThickness: 0.1,
					bevelSize: 0.05,
					bevelSegments: 5
				});

				textGeometry.center();

				const textMaterial = new THREE.MeshPhongMaterial({
					color: 0xff1493,
					specular: 0xff69b4,
					shininess: 100
				});

				textMesh = new THREE.Mesh(textGeometry, textMaterial);
				scene.add(textMesh);

				// Create roses after text is loaded
				createRoses();

				// Start animation
				animate();
			},
			function (xhr) {
				const percent = ((xhr.loaded / xhr.total) * 100).toFixed(0);
				document.getElementById('loading').textContent = `Loading font... ${percent}%`;
			},
			function (err) {
				console.error('Font loading error:', err);
				document.getElementById('loading').style.display = 'none';
				const errorDiv = document.getElementById('error');
				errorDiv.style.display = 'block';
				errorDiv.textContent = 'Failed to load font. Please refresh the page.';
			}
		);

		// Animation loop
		function animate() {
			animationId = requestAnimationFrame(animate);

			// Rotate text
			if (textMesh) {
				textMesh.rotation.y += 0.01;
				// Floating animation
				floatOffset += 0.05;
				textMesh.position.y = Math.sin(floatOffset) * 0.3;
			}

			// Update roses
			updateRoses();

			// Render scene
			renderer.render(scene, camera);
		}

		// Handle window resize
		resizeHandler = function () {
			camera.aspect = window.innerWidth / window.innerHeight;
			camera.updateProjectionMatrix();
			renderer.setSize(window.innerWidth, window.innerHeight);
			renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
		};
		window.addEventListener('resize', resizeHandler);
	}

	// Prevent touch scrolling and zooming on mobile
	function preventTouchMove(e) {
		e.preventDefault();
	}

	function preventGesture(e) {
		e.preventDefault();
	}
</script>

<svelte:head>
	<title>I Love Amina So Much</title>
</svelte:head>

<div class="page-wrapper">
	<div id="loading">Loading...</div>
	<div id="error"></div>
	<div id="canvas-container"></div>
</div>

<style>
	.page-wrapper {
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		margin: 0;
		overflow: hidden;
		background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
		font-family: Arial, sans-serif;
		z-index: 0;
	}

	#canvas-container {
		width: 100vw;
		height: 100vh;
		will-change: transform;
		touch-action: none;
	}

	#loading {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		color: white;
		font-size: 24px;
		text-align: center;
		z-index: 10;
	}

	#error {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		color: #ff6b6b;
		font-size: 18px;
		text-align: center;
		display: none;
		z-index: 10;
	}
</style>
