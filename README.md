# bounce
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>LEVEL 2</title>

    <script type="text/javascript" src="lib/three.js"></script>
    <script type="text/javascript" src="lib/ShadowMapViewer.js"></script>
    <script type="text/javascript" src="lib/HDRCubeTextureLoader.js"></script>
    <script type="text/javascript" src="lib/dat.gui.min.js"></script>
    <script type="text/javascript" src="lib/DragControls.js"></script>
    <script type="text/javascript" src="lib/TrackballControls.js"></script>
    <script type="text/javascript" src="lib/OBJLoader.js"></script>
    <script type="text/javascript" src="lib/LegacyJSONLoader.js"></script>
    <script type="text/javascript" src="lib/Tween.js"></script>
    <script src="lib/FirstPersonControls.js"></script>
    <script src="lib/Detector.js"></script>
    <script src="js/THREEx.KeyboardState.js"></script>

    <script src="lib/DDSLoader.js"></script>
    <script src="lib/MTLLoader.js"></script>
    <script src="lib/OBJLoader.js"></script>

    <script src="js/jquery-1.9.1.js"></script>
    <script src="js/jquery-ui.js"></script>
    <link rel=stylesheet href="css/jquery-ui.css" />
    <link rel=stylesheet href="css/info.css"/>
    <script src="js/info.js"></script>

    <style>

        body {
            /* set margin to 0 and overflow to hidden, to go fullScreen */
            margin: 0;
            overflow: hidden;
        }

    </style>
</head>
<body>


<!-- Div which will hold the Output -->
<div id="WebGL-output">
</div>

<script type="text/javascript">

    //  Height and Width Variables
    var width = window.innerWidth;
    var height = window.innerHeight;
    var ambientLight;
    var scene, camera, renderer, controls;
    var event,objects, obj,sphere;
    var ball,balls = [50],ball2,balls2 = [20];
    var texture;
    var surface;
    var surfaceMesh;
    var group;

    var key = true;
    var keyboard = new THREEx.KeyboardState();

    var collidableMeshList = [];
    var collidableMeshList1 = [];

    init();
    animate();

    function init(){
        // create a scene, that will hold all our elements such as objects, cameras and lights.
        scene = new THREE.Scene();

        //  Cube mapping second method
        var reflectionCube = THREE.ImageUtils.loadTexture('texture/sky.jpg');
        reflectionCube.format = THREE.RGBFormat;
        scene.background = reflectionCube;

        // create a camera, which defines where we're looking at.
        camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 15000);

        // position and point the camera to the center of the scene
        camera.position.x = 7500;
        camera.position.y = 60;
        camera.position.z = 300;
        camera.rotateX(2*Math.PI);


        // create a render and set the size
        renderer = new THREE.WebGLRenderer({antialias: true});
        var dragControls = new THREE.DragControls( objects, camera, renderer.domElement );
        dragControls.addEventListener( 'dragstart', function ( event ) { controls.enabled = false; } );
        dragControls.addEventListener( 'dragend', function ( event ) { controls.enabled = true; } );

        renderer.setClearColor(new THREE.Color(0x49d1ca));
        renderer.setSize(width , height);

        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        document.body.appendChild(renderer.domElement);

        //ambientLight
        ambientLight = new THREE.AmbientLight(0xffffff, 1);
        scene.add(ambientLight);

        //Surface
        texture = THREE.ImageUtils.loadTexture('texture/road.jpg');
        surface = new THREE.PlaneGeometry(15000,500 );
        var surfaceMaterial = new THREE.MeshPhongMaterial({color: 0xffffff, side: THREE.DoubleSide,map:texture});
        surfaceMesh = new THREE.Mesh(surface, surfaceMaterial);
        surfaceMesh.rotateX(Math.PI/2);
        surfaceMesh.position.x = 100;
        surfaceMesh.position.y = -50;
        surfaceMesh.position.z = -40;
        surfaceMesh.receiveShadow = true;
        scene.add(surfaceMesh);

        //Group
        group = new THREE.Group();
        scene.add(group);

        //Balls1
        texture2 = new THREE.TextureLoader().load( 'texture/blue.jpg' );


        texture2.magFilter = THREE.NearestFilter;
        texture2.minFilter = THREE.NearestFilter;

        var ballGeometry = new THREE.SphereGeometry(25, 128, 256);
        var ballMaterial = new THREE.MeshPhongMaterial({
            map: texture2,
            shininess: 10,
            specular: 0xffffff,
            flatShading : true
        });
        ball = new THREE.Mesh(ballGeometry, ballMaterial);
        ball.position.x = -1450;
        ball.position.y = -20;
        ball.position.z = -30;


        //  Clone ball into balls array
        Balls();

        //  Randomly placed the balls in scene
        for (var j = 0; j < balls.length; j++) {


            balls[j].position.x =j*400-1000;
            balls[j].position.y =((Math.random()*2)+3.5)*100;
            balls[j].position.z = -30;
            balls[j].castShadow = true;
            balls[j].receiveShadow = true;
            group.add(balls[j]);
            collidableMeshList.push(balls[j]);
        }

        group.position.set(-1450, -20, -30);
        group.position.y-=2;

        if(group.position.y >= -50){
            group.position.y+=3;
        }

    }
    function animate() {
        var t = (Date.now() / 1000);
        // render the scene
        render();

        requestAnimationFrame(animate);

        camera.position.x-=5;

        group.position.y-=2;

        update();
    }
    function render() {

        //controls.update();
        camera.updateMatrixWorld();
        renderer.render( scene, camera );
    }
    function Balls() {

        for (var i = 0; i < 50; i++) {
            balls[i] = ball.clone();
        }
    }
        function update() {

        if (keyboard.pressed("up")) {

            camera.position.y += 5;

        }

        if (keyboard.pressed("down")) {
            camera.position.y -= 5;


    }


</script>
</body>
</html>
