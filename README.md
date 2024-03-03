// Set up the scene
var scene = new THREE.Scene();
var camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
var renderer = new THREE.WebGLRenderer();
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Create sphere geometry for atoms
var carbonGeometry = new THREE.SphereGeometry(1, 32, 32);
var hydrogenGeometry = new THREE.SphereGeometry(0.5, 32, 32);

// Create materials for atoms
var carbonMaterial = new THREE.MeshPhongMaterial({ color: 0xff0000 }); // Red for carbon
var hydrogenMaterial = new THREE.MeshPhongMaterial({ color: 0x0000ff }); // Blue for hydrogen

// Create sphere mesh for atoms
var carbonAtom = new THREE.Mesh(carbonGeometry, carbonMaterial);
var hydrogenAtoms = [];
for (var i = 0; i < 4; i++) {
    hydrogenAtoms.push(new THREE.Mesh(hydrogenGeometry, hydrogenMaterial));
}

// Position atoms
carbonAtom.position.set(0, 0, 0);
hydrogenAtoms[0].position.set(3, 0, 0);
hydrogenAtoms[1].position.set(-3, 0, 0);
hydrogenAtoms[2].position.set(0, 3, 0);
hydrogenAtoms[3].position.set(0, -3, 0);

// Add atoms to the scene
scene.add(carbonAtom);
hydrogenAtoms.forEach(function(atom) {
    scene.add(atom);
});

// Create cylinder geometry for bonds
var bondGeometry = new THREE.CylinderGeometry(0.1, 0.1, 3, 32);

// Create material for bonds
var bondMaterial = new THREE.MeshPhongMaterial({ color: 0xffffff, emissive: 0xaaaaaa });

// Create cylinder mesh for bonds
var bonds = [];
for (var i = 0; i < 4; i++) {
    var bond = new THREE.Mesh(bondGeometry, bondMaterial);
    bond.position.set(0, 0, 0);
    bonds.push(bond);
}

// Position bonds between atoms
bonds[0].position.set(1.5, 0, 0);
bonds[1].position.set(-1.5, 0, 0);
bonds[2].position.set(0, 1.5, 0);
bonds[3].position.set(0, -1.5, 0);

// Add bonds to the scene
bonds.forEach(function(bond) {
    scene.add(bond);
});

// Add a green-colored plane object
var planeGeometry = new THREE.PlaneGeometry(100, 100);
var planeMaterial = new THREE.MeshPhongMaterial({ color: 0x00ff00 }); // Green for the plane
var plane = new THREE.Mesh(planeGeometry, planeMaterial);
plane.rotation.x = -Math.PI / 2; // Make the plane horizontal
plane.position.y = -5; // Position the plane below the molecule
scene.add(plane);

// Set up lighting
var ambientLight = new THREE.AmbientLight(0xffffff, 0.5);
var directionalLight = new THREE.DirectionalLight(0xffffff, 0.5);
directionalLight.position.set(1, 1, 1);
scene.add(ambientLight);
scene.add(directionalLight);

// Enable animation
function animate() {
    requestAnimationFrame(animate);
    renderer.render(scene, camera);
}

animate();

// Set up camera
camera.position.z = 10;

// Enable mouse controls
var controls = new THREE.OrbitControls(camera, renderer.domElement);
