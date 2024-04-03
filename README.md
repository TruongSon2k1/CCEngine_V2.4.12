
# Modified by pTSern

- Changing some functions of the engine.

## Changed

**CCPhysicsManager:**
<summary> function `update` [172] </summary>
    <details>
    <summary> FROM </summary>

        update: function (dt) {
                var world = this._world;
                if (!world || !this.enabled) return;

                this.emit('before-step');

                this._steping = true;

                var velocityIterations = PhysicsManager.VELOCITY_ITERATIONS;
                var positionIterations = PhysicsManager.POSITION_ITERATIONS;

                if (this.enabledAccumulator) {
                    this._accumulator += dt;

                    var FIXED_TIME_STEP = PhysicsManager.FIXED_TIME_STEP;
                    var MAX_ACCUMULATOR = PhysicsManager.MAX_ACCUMULATOR;

                    // max accumulator time to avoid spiral of death
                    if (this._accumulator > MAX_ACCUMULATOR) {
                        this._accumulator = MAX_ACCUMULATOR;
                    }

                    while (this._accumulator > FIXED_TIME_STEP) {
                        world.Step(FIXED_TIME_STEP, velocityIterations, positionIterations);
                        this._accumulator -= FIXED_TIME_STEP;
                    }
                }
                else {
                    var timeStep = dt;
                    world.Step(timeStep, velocityIterations, positionIterations);
                }

                if (this.debugDrawFlags) {
                    this._checkDebugDrawValid();
                    this._debugDrawer.clear();
                    world.DrawDebugData();
                }

                this._steping = false;

                var events = this._delayEvents;
                for (var i = 0, l = events.length; i < l; i++) {
                    var event = events[i];
                    event.target[event.func].apply(event.target, event.args);
                }
                events.length = 0;

                this._syncNode();
            },

</details>
    <details>
    <summary> TO </summary>

        update: window['__pts_moded_mode__'] == undefined ?
            function (dt) {
                var world = this._world;
                if (!world || !this.enabled) return;

                this.emit('before-step');

                this._steping = true;

                var velocityIterations = PhysicsManager.VELOCITY_ITERATIONS;
                var positionIterations = PhysicsManager.POSITION_ITERATIONS;

                if (this.enabledAccumulator) {
                    this._accumulator += dt;

                    var FIXED_TIME_STEP = PhysicsManager.FIXED_TIME_STEP;
                    var MAX_ACCUMULATOR = PhysicsManager.MAX_ACCUMULATOR;

                    // max accumulator time to avoid spiral of death
                    if (this._accumulator > MAX_ACCUMULATOR) {
                        this._accumulator = MAX_ACCUMULATOR;
                    }

                    while (this._accumulator > FIXED_TIME_STEP) {
                        world.Step(FIXED_TIME_STEP, velocityIterations, positionIterations);
                        this._accumulator -= FIXED_TIME_STEP;
                    }
                }
                else {
                    var timeStep = dt;
                    world.Step(timeStep, velocityIterations, positionIterations);
                }

                if (this.debugDrawFlags) {
                    this._checkDebugDrawValid();
                    this._debugDrawer.clear();
                    world.DrawDebugData();
                }

                this._steping = false;

                var events = this._delayEvents;
                for (var i = 0, l = events.length; i < l; i++) {
                    var event = events[i];
                    event.target[event.func].apply(event.target, event.args);
                }
                events.length = 0;

                this._syncNode();
            }
            :
            function (dt) {
                var world = this._world;
                if (!world || !this.enabled) return;

                this.emit('before-step');

                this._steping = true;

                var velocityIterations = PhysicsManager.VELOCITY_ITERATIONS;
                var positionIterations = PhysicsManager.POSITION_ITERATIONS;

                if (this.enabledAccumulator) {
                    this._accumulator += dt;

                    var FIXED_TIME_STEP = PhysicsManager.FIXED_TIME_STEP;
                    var MAX_ACCUMULATOR = PhysicsManager.MAX_ACCUMULATOR;

                    // max accumulator time to avoid spiral of death
                    if (this._accumulator > MAX_ACCUMULATOR) {
                        this._accumulator = MAX_ACCUMULATOR;
                    }

                    while (this._accumulator > FIXED_TIME_STEP) {
                        world.Step(FIXED_TIME_STEP, velocityIterations, positionIterations);
                        this._accumulator -= FIXED_TIME_STEP;
                    }
                }
                else {
                    var timeStep = dt;
                    world.Step(timeStep, velocityIterations, positionIterations);
                }

                if (this.debugDrawFlags) {
                    this._checkDebugDrawValid();
                    this._debugDrawer.clear();
                    world.DrawDebugData();
                }

                this._steping = false;

                var events = this._delayEvents;
                for (var i = 0, l = events.length; i < l; i++) {
                    var event = events[i];
                    event.target[event.func].apply(event.target, event.args);
                }
                events.length = 0;

                this._syncNode();
            },

    </details>
</details>

**CCRigidBody:**

<summary> properties </summary>
    <details>
    <summary> ADDED </summary>

        sync_position: {
                get: function() {
                    return this._sync_position_;
                },
                set: function() {
                },
                default: false,
                tooltip: CC_DEV && 'i18n:COMPONENT.physics.rigidbody.sync_position'
            },

            sync_rotation: {
                get: function() {
                    return this._sync_rotation_;
                },
                set: function() {
                },
                default: false,
                tooltip: CC_DEV && 'i18n:COMPONENT.physics.rigidbody.sync_rotation'
            },

</details>
    <details>
    <summary> function `update` </summary>

        update: function() {
            if(this.sync_position) this.syncPosition(this.type === BodyType.Animated)
            if(this.sync_rotation) this.syncRotation(this.type === BodyType.Animated)
        },

</details>

**CCComponent:**

<summary> function [224] </summary>
    <details>
    <summary> ADDED </summary>

        onChange: null,

        _executeOnChange() {
            this.onChange && this.onChange();
        },

</details>

**CCRendererComponent:**

<summary> set function [69] </summary>
    <details>
    <summary> FROM </summary>

        materials: {
            get () {
                return this._materials;
            },
            set (val) {
                this._materials = val;
                this._activateMaterial();
            },
            type: [Material],
            displayName: 'Materials',
            animatable: false
        }

</details>
    <details>
    <summary> TO </summary>

        materials: {
            get () {
                return this._materials;
            },
            set (val) {
                this._materials = val;
                this._activateMaterial();
                this._executeOnChange();
            },
            type: [Material],
            displayName: 'Materials',
            animatable: false
        }

</details>


