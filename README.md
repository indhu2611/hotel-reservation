import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Scanner;
class Room {
    private int roomNumber;
    private String roomType;
    private double price;
    private boolean isAvailable;

    public Room(int roomNumber, String roomType, double price, boolean isAvailable) {
        this.roomNumber = roomNumber;
        this.roomType = roomType;
        this.price = price;
        this.isAvailable = isAvailable;
    }

    // Getters and Setters
    public int getRoomNumber() { return roomNumber; }
    public String getRoomType() { return roomType; }
    public double getPrice() { return price; }
    public boolean isAvailable() { return isAvailable; }
    public void setAvailable(boolean available) { isAvailable = available; }

    @Override
    public String toString() {
        return "Room " + roomNumber + " (" + roomType + ", $" + price + ")";
    }
}

// Reservation class
class Reservation {
    private Room room;
    private Date checkInDate;
    private Date checkOutDate;
    private String customerName;

    public Reservation(Room room, Date checkInDate, Date checkOutDate, String customerName) {
        this.room = room;
        this.checkInDate = checkInDate;
        this.checkOutDate = checkOutDate;
        this.customerName = customerName;
        room.setAvailable(false); // Mark the room as unavailable
    }

    @Override
    public String toString() {
        return "Reservation for " + customerName + " in " + room.toString() +
                " from " + checkInDate + " to " + checkOutDate;
    }
}

// Payment class
class Payment {
    public static boolean processPayment(double amount) {
        System.out.println("Processing payment of $" + amount + "...");
        return true; // Assume payment is always successful
    }
}

// HotelReservationSystem class
class HotelReservationSystem {
    private List<Room> rooms = new ArrayList<>();
    private List<Reservation> reservations = new ArrayList<>();

    public void addRoom(Room room) { rooms.add(room); }

    public List<Room> searchAvailableRooms(String roomType) {
        List<Room> availableRooms = new ArrayList<>();
        for (Room room : rooms) {
            if (room.isAvailable() && room.getRoomType().equalsIgnoreCase(roomType)) {
                availableRooms.add(room);
            }
        }
        return availableRooms;
    }

    public void makeReservation(String customerName, String roomType, Date checkIn, Date checkOut) {
        List<Room> availableRooms = searchAvailableRooms(roomType);
        if (!availableRooms.isEmpty()) {
            Room selectedRoom = availableRooms.get(0); // Pick the first available room
            if (Payment.processPayment(selectedRoom.getPrice())) {
                Reservation reservation = new Reservation(selectedRoom, checkIn, checkOut, customerName);
                reservations.add(reservation);
                System.out.println("Reservation successful! Details: " + reservation);
            } else {
                System.out.println("Payment failed. Reservation not created.");
            }
        } else {
            System.out.println("No available rooms of type: " + roomType);
        }
    }

    public void viewReservations() {
        if (reservations.isEmpty()) {
            System.out.println("No reservations found.");
        } else {
            for (Reservation reservation : reservations) {
                System.out.println(reservation);
            }
        }
    }

    public static void main(String[] args) {
        HotelReservationSystem system = new HotelReservationSystem();
        system.addRoom(new Room(101, "Single", 100, true));
        system.addRoom(new Room(102, "Double", 150, true));
        system.addRoom(new Room(201, "Suite", 250, true));

        Scanner scanner = new Scanner(System.in);
        System.out.println("Welcome to the Hotel Reservation System!");

        System.out.print("Enter customer name: ");
        String customerName = scanner.nextLine();
        System.out.print("Enter room type (Single/Double/Suite): ");
        String roomType = scanner.nextLine();

        Date checkIn = new Date(); // Simplified for demo purposes
        Date checkOut = new Date(); // Simplified for demo purposes

        system.makeReservation(customerName, roomType, checkIn, checkOut);
        system.viewReservations();
    }
}
