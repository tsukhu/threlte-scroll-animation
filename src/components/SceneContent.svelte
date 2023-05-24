<script lang="ts">
	import { T, useRender, useThrelte } from '@threlte/core';
	import { Color, Quaternion, Vector3 } from 'three';
	import { scrollPercent } from '../stores/scrollStore';
	const { camera } = useThrelte();
	let color = new Color(0x00ff00);
	let cubePos = new Vector3(0, 0.5, -10);
	let cubeRotation = new Quaternion();

	function lerp(x: number, y: number, a: number): number {
		return (1 - a) * x + a * y;
	}

	function scalePercent(start: number, end: number) {
		return ($scrollPercent - start) / (end - start);
	}

	const animationScripts: { start: number; end: number; func: () => void }[] = [];

	//add an animation that flashes the cube through 100 percent of scroll
	animationScripts.push({
		start: 0,
		end: 101,
		func: () => {
			let g = color.g;
			g -= 0.05;
			if (g <= 0) {
				g = 1.0;
			}
			color.g = g;
		}
	});

	//add an animation that moves the cube through first 40 percent of scroll
	animationScripts.push({
		start: 0,
		end: 40,
		func: () => {
			camera.current.lookAt(cubePos);
			camera.current.position.set(0, 1, 2);
			cubePos.z = lerp(-10, 0, scalePercent(0, 40));
		}
	});

	//add an animation that rotates the cube between 40-60 percent of scroll
	animationScripts.push({
		start: 40,
		end: 60,
		func: () => {
			camera.current.lookAt(cubePos);
			camera.current.position.set(0, 1, 2);
			cubeRotation.z = lerp(0, Math.PI, scalePercent(40, 60));
		}
	});

	//add an animation that moves the camera between 60-80 percent of scroll
	animationScripts.push({
		start: 60,
		end: 80,
		func: () => {
			camera.current.position.x = lerp(0, 5, scalePercent(60, 80));
			camera.current.position.y = lerp(1, 5, scalePercent(60, 80));
			camera.current.lookAt(cubePos);
		}
	});

	//add an animation that auto rotates the cube from 80 percent of scroll
	animationScripts.push({
		start: 80,
		end: 101,
		func: () => {
			//auto rotate
			cubeRotation.x += 0.01;
			cubeRotation.y += 0.01;
		}
	});

	function playScrollAnimations() {
		animationScripts.forEach((a) => {
			if ($scrollPercent >= a.start && $scrollPercent < a.end) {
				a.func();
			}
		});
	}

	useRender(({ camera, renderer, scene }, delta) => {
		playScrollAnimations();
		if (renderer) renderer.render(scene, camera.current);
	});
</script>

<T.PerspectiveCamera fov={75} near={0.1} far={1000} />
<T.GridHelper size={10} division={10} colorCenterLine={'0xaec6cf'} colorGrid="0xaec6cf" />
<T.Mesh
	position={[cubePos.x, cubePos.y, cubePos.z]}
	rotation={[cubeRotation.x, cubeRotation.y, cubeRotation.z]}
>
	<T.BoxGeometry args={[1, 2, 1]} />
	<T.MeshBasicMaterial {color} wireframe />
</T.Mesh>
