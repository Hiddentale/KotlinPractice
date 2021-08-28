import java.util.Scanner

var created = false
var parkingSpaces : ParkingObject? = null

class ParkingObject(parkingSize: Int = 1) {
    var parkSpace: MutableList<String> = MutableList(parkingSize) { " " }
}

fun regByColor(color: String) {
    var switch = false
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else {
        for (car in parkingSpaces?.parkSpace!!) {
            if (car.contains(color)) {
                if (!switch) {
                    switch = true
                    print(car.split(" ")[0])
                } else {
                    print(", ${car.split(" ")[0]}")
                }
            }
        }
        if (switch) {
            println()
        }
        else {
            println("No cars with color $color were found.")
        }
    }
}

fun spotByColor(color: String) {
    var switch = false
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else {
        for (car in parkingSpaces?.parkSpace!!) {
            if (car.contains(color)) {
                if (!switch) {
                    switch = true
                    print(parkingSpaces?.parkSpace?.indexOf(car)?.plus(1))
                } else {
                    print(", ${parkingSpaces?.parkSpace?.indexOf(car)?.plus(1)}")
                }
            }
        }
        if (switch) {
            println()
        }
        else {
            println("No cars with color $color were found.")
        }
    }
}

fun spotByReg(reg: String) {
    var switch = false
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else {
        for (car in parkingSpaces?.parkSpace!!) {
            if (car.contains(reg)) {
                switch = true
                println(parkingSpaces?.parkSpace?.indexOf(car)?.plus(1))
                break
            }
        }
        if (!switch) {
            println("No cars with registration number $reg were found.")
        }
    }

}

fun create(size: Int) {
    if (size <= 0) {
        println("Dimensional-technology to create such parking lots does not exist yet.")
    } else {
        println("Created a parking lot with $size spots.")
        parkingSpaces = ParkingObject(size)
        created = true
    }
}

fun park(license: String, color: String) {
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else if (parkingSpaces?.parkSpace?.firstOrNull { it == " "} != null) {
        val parkingNumber = parkingSpaces?.parkSpace?.indexOf(parkingSpaces?.parkSpace?.find { it == " "})
        parkingSpaces?.parkSpace!![parkingNumber!!] = "$license $color"
        println("$color car parked in spot ${parkingNumber + 1}.")
    } else {
        println("Sorry, the parking lot is full.")
    }
}

fun leave(num: Int) {
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else if (parkingSpaces?.parkSpace!![num - 1] != " ") {
        parkingSpaces?.parkSpace!![num - 1] = " "
        println("Spot $num is free.")
    } else {
        println("There is no car in this spot.")
    }
}

fun status() {
    if (!created) {
        println("Sorry, a parking lot has not been created.")
    } else if (parkingSpaces?.parkSpace?.filter { it == " " }?.size == parkingSpaces?.parkSpace?.size) {
        println("Parking lot is empty.")
    } else {
        for (element in parkingSpaces?.parkSpace?.filterNot { it == " " }!!) {
            println("${parkingSpaces?.parkSpace?.indexOf(element)?.plus(1)} $element")
        }
    }
}

fun main() {
    val scanner = Scanner(System.`in`)
    while (true) {
        val input = scanner.nextLine()
        val lines = input.split(" ")
        when(lines[0]) {
            "park" -> park(lines[1], lines[2].lowercase())
            "leave" -> leave(lines[1].toInt())
            "status" -> status()
            "create" -> create(lines[1].toInt())
            "reg_by_color" -> regByColor(lines[1].lowercase())
            "spot_by_color" -> spotByColor(lines[1].lowercase())
            "spot_by_reg" -> spotByReg(lines[1])
            "exit" -> break
            else -> println("That input does not equate to anything useful.")
        }
    }
}
