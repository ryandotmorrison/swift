// Try using different emoji—for example, colorful flags.
let emoji = "🌕🎾⚾️🌍⚽️"
// Try changing the number of dancers.
let numberOfDancers = 200

// Draw: As you touch and drag around, this function is called each time your touch moves, adding emoji to the scene.
func addEmoji(touch: Touch) {
    if touch.previousPlaceDistance < 100 { return }
    let characters = emoji.componentsByCharacter()
    let graphic = Graphic(text: characters.randomItem)
    scene.place(graphic, at: touch.position)
}

// Kaleido: As you touch and drag around, this function is called each time your touch moves, adding emoji to the scene in symmetrical patterns.
func addKaleidoImage(touch: Touch) {
    let characters = emoji.componentsByCharacter()
    // Space out graphics.
    let placeDistance = touch.position.distance(from: lastPlacePosition)
    if placeDistance < 80 { return }
    lastPlacePosition = touch.position
    // Create graphics for each quadrant and add to an array.
    var graphics: [Graphic] = []
    for i in 0 ..< 4 {
        let graphic = Graphic(text: characters.randomItem)
        graphics.append(graphic)
    }
    // Get absolute x, y values.
    let x = abs(touch.position.x)
    let y = abs(touch.position.y)
    // Position a graphic in each quadrant.
    let position1 = Point(x: x, y: y)
    scene.place(graphics[0], at: position1)
    let position2 = Point(x: x, y: -y)
    scene.place(graphics[1], at: position2)
    let position3 = Point(x: -x, y: -y)
    scene.place(graphics[2], at: position3)
    let position4 = Point(x: -x, y: y)
    scene.place(graphics[3], at: position4)
}

// Play: When you touch an emoji, this function is called to play a note on the cosmic drums.
func playNote(graphic: Graphic) {
    // Number of notes.
    let noteSteps = 16
    // Distance from the center 0.0 - 1.0.
    let center = Point(x: 0, y: 0)
    let distance = graphic.distance(from: center) / 707.0
    // Translate distance into closest note Int value.
    let note = (1.0 - distance) * noteSteps
    // Translate alpha into volume value.
    let volume = graphic.alpha * 100.0
    // Play the note.
    playInstrument(.cosmicDrums, note: note, volume: volume)
}

// Hello: When you touch an emoji, this function is called to speak or make a sound.
func sayHello(graphic: Graphic) {
    // Choose what to do based on emoji.
    switch graphic.text {
    case "🌕":
        speak("Planet smelly cheese.")
    case "🎾":
        playSound(.tennis)
    case "⚾️":
        playSound(.wap)
    case "🌍":
        speak("Blue marble.")
    case "⚽️":
        speak("Planet soccer ball.")
    default:
        speak(graphic.text)
    }
}

// Goodbye: When you touch an emoji, this function is called to make a graphic disappear.
func fadeAway(graphic: Graphic) {
    playSound(.tumble)
    let sinkHole = Point(x: graphic.position.x, y: -800)
    graphic.moveAndRemove(to: sinkHole, duration: 2.5)
    graphic.fadeOut(after: 2.0)
}

// Astrodance: When you press the button, this function rearranges emoji into lovely patterns.
func dance() {
    // Limit the number of dancers.
    let maximumNumberOfDancers = 500
    let actualNumberOfDancers = min(numberOfDancers, maximumNumberOfDancers)
    // Place the dancers on the scene, if necessary.
    if scene.graphics.count != actualNumberOfDancers {
        scene.clear()
        dancers = []
        var index = 0
        let emojiCharacters = emoji.componentsByCharacter()
        for i in 0..<actualNumberOfDancers {
            let dancer = Graphic(text: emojiCharacters[index])
            scene.place(dancer, at: center)
            dancers.append(dancer)
            index += 1
            if index == emojiCharacters.count {
                index = 0
            }
        }
    }
    // Get a new pattern.
    let pattern = randomInt(from: 1, to: 3)
    // Rearrange the dancers.
    rearrange(dancers: dancers, pattern: pattern, instrument: .cosmicDrums)
}

// Rearrange the dancers to a pattern accompanied by an instrument.
func rearrange(dancers: [Graphic], pattern: Int, instrument: Instrument.Kind) {
    
    var points = [Point]()
    var note = 5
    
    // Start with all points in the center.
    for i in 0..<dancers.count {
        points.append(center)
    }
    
    // Get a new set of points depending on the pattern:
    // Pattern 0: Points in the center.
    // Pattern 1: Points in a circle.
    if pattern == 1 {
        let radius = randomDouble(from: 100, to: 450)
        points = scene.circlePoints(radius: radius, count: dancers.count)
        note = 14
    }
    // Pattern 3: Points in a spiral.
    if pattern == 2 {
        let spacing = randomDouble(from: 50, to: 200)
        points = scene.spiralPoints(spacing: spacing, count: dancers.count)
        note = 10
    }
    // Pattern 4: Hypotrochoid.
    if pattern == 3 {
        let r1 = randomDouble(from: 200.0, to: 400.0)
        let r2 = randomDouble(from: 50.0, to: 100.0)
        let d = randomDouble(from: 0.5, to: 2.0)
        points = scene.hypotrochoidPoints(r1: r1, r2: r2, d: d, count: dancers.count)
        note = 12
    }

    // Shift all points by a random x, y distance.
    let dx = randomDouble(from: -250, to: 250)
    let dy = randomDouble(from: -250, to: 250)
    
    for i in 0..<dancers.count {
        points[i].x += dx
        points[i].y += dy
    }

    // Get random values.
    let duration = randomDouble(from: 0.25, to: 2.0)
    let rotation = randomDouble(from: -360, to: 360)
    
    // Animate dancers to their new positions.
    for i in 0..<dancers.count {
        dancers[i].move(to: points[i], duration: duration)
        // Scale based on the distance from the center.
        let distance = points[i].distance(from: center) / 707.0
        dancers[i].scale = 0.25 + (distance * 2.0)
        dancers[i].rotation = rotation
    }
    
    // Play a note on the instrument.
    playInstrument(instrument, note: note, volume: 50)
}

// Create and add the Draw button.
let emojiTool = Tool(name: "Draw", emojiIcon: "🖌")
emojiTool.onTouchMoved = addEmoji(touch:)
scene.tools.append(emojiTool)

// Create and add the Kaleido button.
let kaleidoTool = Tool(name: "Kaleido", emojiIcon: "❄️")
kaleidoTool.onTouchMoved = addKaleidoImage(touch:)
scene.tools.append(kaleidoTool)

// Create and add the Play button.
let playTool = Tool(name: "Play", emojiIcon: "🎶")
playTool.onGraphicTouched = playNote(graphic:)
scene.tools.append(playTool)

// Create and add the Hello button.
let helloTool = Tool(name: "Hello", emojiIcon: "🗣")
helloTool.onGraphicTouched = sayHello(graphic:)
scene.tools.append(helloTool)

// Create and add the Goodbye button.
let goodbyeTool = Tool(name: "Goodbye", emojiIcon: "⚡️")
goodbyeTool.onGraphicTouched = fadeAway(graphic:)
scene.tools.append(goodbyeTool)

// Create and connect the Dance button.
let danceButton = Button(name: "Astrodance")
danceButton.onPress = dance
scene.button = danceButton
