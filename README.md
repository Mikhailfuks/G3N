package main

import ( "fmt","log")

    "github.com/g3n/engine/geometry"
    "github.com/g3n/engine/graphic"
    "github.com/g3n/engine/light"
    "github.com/g3n/engine/material"
    "github.com/g3n/engine/math32"
    "github.com/g3n/engine/renderer"
    "github.com/g3n/engine/scene"
    "github.com/g3n/engine/window"
)

func main() {
    // Create a new window.
    win, err := window.Create(window.Options{
        Title: "G3N Example",
        Width: 1024,
        Height: 768,
    })
    if err != nil {
        log.Fatal(err)
    }

    // Create a new scene.
    scn := scene.New()

    // Create a new camera.
    cam := scene.NewCameraPerspective(math32.NewDegToRad(45), float32(win.Width())/float32(win.Height()), 1, 100)
    cam.SetPosition(0, 1, 5)
    scn.Add(cam)

    // Create a new ambient light.
    amb := light.NewAmbient(math32.NewColor("white"), 0.5)
    scn.Add(amb)

    // Create a new directional light.
    dir := light.NewDirectional(math32.NewColor("white"), 1)
    dir.SetPosition(1, 2, 1)
    scn.Add(dir)

    // Create a new cube geometry.
    cubeGeo := geometry.NewCube(1)

    // Create a new material.
    cubeMat := material.NewStandard(math32.NewColor("red"))

    // Create a new mesh.
    cubeMesh := graphic.NewMesh(cubeGeo, cubeMat)
    cubeMesh.SetPosition(0, 0, 0)
    scn.Add(cubeMesh)

    // Create a new sphere geometry.
    sphereGeo := geometry.NewSphere(1, 32, 32)

    // Create a new material.
    sphereMat := material.NewStandard(math32.NewColor("blue"))

    // Create a new mesh.
    sphereMesh := graphic.NewMesh(sphereGeo, sphereMat)
    sphereMesh.SetPosition(2, 0, 0)
    scn.Add(sphereMesh)

    // Create a new renderer.
    rnd := renderer.New(renderer.Options{
        Window: win,
    })

    // Run the main loop.
    for !win.ShouldClose() {
        // Update the scene.
        scn.Update(win.DeltaTime())

        // Render the scene.
        rnd.Render(scn, cam)

        // Swap the buffers.
        win.SwapBuffers()

        // Poll for events.
        win.PollEvents()
    }

    // Close the window.
    win.Close()
}
