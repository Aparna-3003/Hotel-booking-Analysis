# Hotel-booking-Analysis

import java.util.ArrayList;
import java.util.Scanner;

class Booking {

    int bookingId;
    String customerName;
    int rooms;
    int days;
    double pricePerRoom;
    double totalAmount;
    boolean isCancelled;

    Booking(int bookingId, String customerName,
            int rooms, int days, double pricePerRoom) {

        this.bookingId = bookingId;
        this.customerName = customerName;
        this.rooms = rooms;
        this.days = days;
        this.pricePerRoom = pricePerRoom;

        totalAmount = rooms * days * pricePerRoom;
        isCancelled = false;
    }

    void displayBooking() {

        System.out.println("\nBooking ID: " + bookingId);
        System.out.println("Customer Name: " + customerName);
        System.out.println("Rooms: " + rooms);
        System.out.println("Days: " + days);
        System.out.println("Price Per Room: Rs. " + pricePerRoom);
        System.out.println("Total Amount: Rs. " + totalAmount);

        if (isCancelled) {
            System.out.println("Status: Cancelled");
        } else {
            System.out.println("Status: Active");
        }
    }
}

public class Main {

    static Scanner sc = new Scanner(System.in);

    static ArrayList<Booking> bookings = new ArrayList<>();

    static int nextBookingId = 1001;

    public static void main(String[] args) {

        int choice;

        do {

            System.out.println("\n===== HOTEL BOOKING SYSTEM =====");
            System.out.println("1. Book Hotel Room");
            System.out.println("2. Cancel Booking");
            System.out.println("3. Booking History");
            System.out.println("4. Search Booking by ID");
            System.out.println("5. Booking Analysis");
            System.out.println("6. Exit");

            System.out.print("Enter your choice: ");
            choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {

                case 1:
                    bookRoom();
                    break;

                case 2:
                    cancelBooking();
                    break;

                case 3:
                    bookingHistory();
                    break;

                case 4:
                    searchBooking();
                    break;

                case 5:
                    bookingAnalysis();
                    break;

                case 6:
                    System.out.println("Thank You!");
                    break;

                default:
                    System.out.println("Invalid Choice!");
            }

        } while (choice != 6);

        sc.close();
    }

    // Book Hotel Room

    static void bookRoom() {

        System.out.print("Enter Customer Name: ");
        String name = sc.nextLine();

        System.out.print("Enter Number of Rooms: ");
        int rooms = sc.nextInt();

        System.out.print("Enter Number of Days: ");
        int days = sc.nextInt();

        System.out.print("Enter Price Per Room: ");
        double price = sc.nextDouble();

        if (name.trim().isEmpty() ||
                rooms <= 0 || days <= 0 || price <= 0) {

            System.out.println("Invalid Booking Details!");
            return;
        }

        Booking b = new Booking(
                nextBookingId,
                name,
                rooms,
                days,
                price
        );

        bookings.add(b);

        System.out.println("\nBooking Successful!");
        System.out.println("Booking ID: " + nextBookingId);
        System.out.println("Customer Name: " + name);
        System.out.println("Total Amount: Rs. " + b.totalAmount);

        nextBookingId++;
    }

    // Cancel Booking

    static void cancelBooking() {

        System.out.print("Enter Booking ID: ");
        int id = sc.nextInt();

        Booking b = findBooking(id);

        if (b == null) {

            System.out.println("Booking Not Found!");

        } else if (b.isCancelled) {

            System.out.println("Booking Already Cancelled!");

        } else {

            b.isCancelled = true;

            System.out.println("Booking Cancelled Successfully!");
            System.out.println("Booking ID: " + b.bookingId);
            System.out.println("Customer Name: " + b.customerName);
        }
    }

    // Booking History

    static void bookingHistory() {

        if (bookings.isEmpty()) {

            System.out.println("No Booking Records Found!");
            return;
        }

        System.out.println("\n===== BOOKING HISTORY =====");

        for (Booking b : bookings) {

            b.displayBooking();
        }
    }

    // Search Booking

    static void searchBooking() {

        System.out.print("Enter Booking ID: ");
        int id = sc.nextInt();

        Booking b = findBooking(id);

        if (b == null) {

            System.out.println("Booking Not Found!");

        } else {

            System.out.println("\n===== BOOKING DETAILS =====");
            b.displayBooking();
        }
    }

    // Booking Analysis

    static void bookingAnalysis() {

        int totalBookings = bookings.size();
        int cancelledBookings = 0;
        int activeBookings = 0;

        double revenue = 0;

        for (Booking b : bookings) {

            if (b.isCancelled) {

                cancelledBookings++;

            } else {

                activeBookings++;
                revenue += b.totalAmount;
            }
        }

        double cancellationRate = 0;

        if (totalBookings > 0) {

            cancellationRate =
                    (cancelledBookings * 100.0) / totalBookings;
        }

        System.out.println("\n===== BOOKING ANALYSIS =====");

        System.out.println("Total Bookings: " + totalBookings);

        System.out.println("Active Bookings: " + activeBookings);

        System.out.println("Cancelled Bookings: "
                + cancelledBookings);

        System.out.println("Cancellation Rate: "
                + cancellationRate + "%");

        System.out.println("Total Revenue: Rs. " + revenue);
    }

    // Find Booking Using ID

    static Booking findBooking(int id) {

        for (Booking b : bookings) {

            if (b.bookingId == id) {

                return b;
            }
        }

        return null;
    }
}
