# RAPIER-Phaser Integration Library

This library allows you to easily integrate RAPIER physics with Phaser 3, making it simple to create and manage rigid bodies within a physical world in Phaser.

## Features
- Support for different types of rigid bodies: Dynamic, fixed, kinematic position-based, or velocity-based.
- Phaser transformations: Optional integration to keep Phaser transformations (position and rotation) synchronized with physical bodies.
- Support for various collider shapes: Boxes, spheres, capsules, triangles, etc.
- Debugging: Enable visual collider rendering for easier debugging.

[Examples](https://phaser.io/sandbox/full/GcQ31KLH)

![Example](https://i.gyazo.com/0e03de1292b8415ffdb4c571c87e081b.png)

## Installation
First, install the required dependencies:
```bash
npm i rapier-connector
```

## Usage
Creating the Physics World
To get started, you need to create a RAPIER physics world within your Phaser scene:

```js
import { RAPIER, createRapierPhysics } from 'rapier-connector';

class MyScene extends Phaser.Scene
{
    constructor()
    {
        super({ key: 'MyScene' });
    }

    preload()
    {
        // Load your assets here
    }

    async create()
    {
        // Initialize RAPIER
        await RAPIER.init();

        // Create the physics world with gravity
        const gravity = new RAPIER.Vector2({ x: 0, y: 400 });
        this.rapierPhysics = createRapierPhysics(gravity, this);

        // Enable debugging (optional)
        this.rapierPhysics.debugger(true);
    }

    update() {
        // Scene update logic
    }
}
```

### Creating Rigid Bodies
You can add rigid bodies to any Phaser game object as follows:

```js
import { RAPIER, createRapierPhysics } from 'rapier-connector';

class MyScene extends Phaser.Scene {
    ...
    async create()
    {
        // Initialize RAPIER
        await RAPIER.init();

        // Create the physics world with gravity
        const gravity = new RAPIER.Vector2({ x: 0, y: 400 });
        this.rapierPhysics = createRapierPhysics(gravity, this);

        const phaserImage = this.add.image(400, 300, 'image');
        
        // Add rigid body with collider shapeType (shape collision are automatically created with the same size as the game object)
        this.phaserImagephysics = this.rapierPhysics.addRigidBody(phaserImage, {
            rigidBodyType: RAPIER.RigidBodyType.Dynamic,  // Rigid body type [fixed | dynamic | kinematicVelocityBased | kinematicPositionBased]
            collider: RAPIER.ShapeType.Ball,  // Collider shape type or colliderDesc
        });

        // Add rigid body with colliderDesc
        this.phaserImagephysics = this.rapierPhysics.addRigidBody(phaserImage, {
            rigidBodyType: RAPIER.RigidBodyType.Dynamic,
            collider: RAPIER.ColliderDesc.cuboid(halfWidth, halfHeight),  // Custom collider shape
        });

        // Add rigid body with Phaser transformations enabled
        this.phaserImagephysics = this.rapierPhysics.addRigidBody(phaserImage, {
            rigidBodyType: RAPIER.RigidBodyType.kinematicPositionBased,
            collider: RAPIER.ShapeType.Ball,
            phaserTransformations: true,
        });

    }
}
```

All rigid bodies are automatically synchronized with the Phaser game object's position and rotation. However, the phaserTransformations option only works if the rigid body is of type KinematicPositionBased. You can enable or disable Phaser transformations by setting the phaserTransformations option to true or false.

The addRigidBody method returns an object with the rigid body and collider associated with the game object. You can use this object to access the rigid body and collider properties, just as if they were created using native RAPIER methods. For more details, please refer to the official documentation: RAPIER JavaScript User Guide on Rigid Bodies.

## Available Methods
`createRapierPhysics(gravity, scene)`  
Creates and manages a RAPIER physics world in a Phaser scene.  
- gravity: Gravity vector (x, y).
- scene: The Phaser scene.

`addRigidBody(gameObject, options)`  
Returns an object with the rigid body and collider associated with the game object.
- gameObject: The Phaser game object to which the rigid body will be added.
- options: Optional configuration for the rigid body, including the body type, collider, and whether Phaser transformations are enabled.
```ts
type TRapierOptions = {
    /** The type of rigidbody (Dynamic, Fixed, KinematicPositionBased, KinematicVelocityBased) */
    rigidBodyType?: RAPIER.RigidBodyType;
    /**
     * The collider shape, if you pass RAPIER.ColliderDesc.[ball | capsule | cuboid | ...] you need pass the shape size example: RAPIER.ColliderDesc.ball(1.5)
     * - If you don't pass a collider, a cuboid will be created with the dimensions of the game object.
     * - If you pass the type enum RAPIER.ShapeType, the size is created with the dimensions of the object.
     * */
    collider?: RAPIER.ColliderDesc | RAPIER.ShapeType;
    /** If you pass some KinematicPositionBased then you can use Phaser's transformations. NOTE: Phaser transformations are only available for KinematicPositionBased rigid bodies. Scale is not supported please do it manually  */
    phaserTransformations?: boolean;
};
```

`debugger(enable)`   
- enable: Enables or disables visual debugging of colliders.

`destroy(gameObject)`  
Destroys the game object and its rigid body, preventing memory leaks.
- gameObject: The game object that will be destroyed along with its rigid body.

## Oficial Documentation
- [RAPIER JavaScript User Guide](https://rapier.rs/docs/user_guides/javascript/getting_started_js)

## Note
If this library does not provide everything you need to develop with RAPIER, please consider using RAPIER natively. RAPIER is already integrated into the library and can be imported directly.

## Join the Phaser Community!

We love to see what developers like you create with Phaser! It really motivates us to keep improving. So please join our community and show-off your work 😄

**Visit:** The [Phaser website](https://phaser.io) and follow on [Phaser Twitter](https://twitter.com/phaser_)<br />
**Play:** Some of the amazing games [#madewithphaser](https://twitter.com/search?q=%23madewithphaser&src=typed_query&f=live)<br />
**Learn:** [API Docs](https://newdocs.phaser.io), [Support Forum](https://phaser.discourse.group/) and [StackOverflow](https://stackoverflow.com/questions/tagged/phaser-framework)<br />
**Discord:** Join us on [Discord](https://discord.gg/phaser)<br />
**Code:** 2000+ [Examples](https://labs.phaser.io)<br />
**Read:** The [Phaser World](https://phaser.io/community/newsletter) Newsletter<br />

Created by [Phaser Studio](mailto:support@phaser.io). Powered by coffee, anime, pixels and love.

The Phaser logo and characters are &copy; 2011 - 2024 Phaser Studio Inc.

All rights reserved.