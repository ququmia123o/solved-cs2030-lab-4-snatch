Download Link: https://assignmentchef.com/product/solved-cs2030-lab-4-snatch
<br>



<h1>Task Content</h1>

<table width="680">

 <tbody>

  <tr>

   <td width="680"><strong>Snatch!</strong><strong>Topic Coverage</strong>Abstract Classes / InterfacesInheritance vs CompositionDesign Principles<a href="https://www.comp.nus.edu.sg/~cs2030/style/">CS2030 Java St</a><a href="https://www.comp.nus.edu.sg/~cs2030/style/">y</a><a href="https://www.comp.nus.edu.sg/~cs2030/style/">le Guide</a><strong>Problem Description</strong><em>Snatch</em> is yet another transport service provider trying to vie for a place in the public transport arena.<em>Snatch</em> provides three types of ride services:JustRideFare is based on the distance @ 22 cents per kmFare is the same regardless of the number of passengers (pax) There is no booking fee.A surcharge of 500 cents if a ride request is issued between the peak hour of 600 hrs to 900 hrs, both inclusiveFare is based on the distance @ 33 cents per km, but there is a booking fee of 200 cents Fare is the same regardless of the number of passengers (pax)No peak hour surchargeShareARideFare is based on the distance @ 50 cents per kmFare is divided equally among the number of passengers There is no booking fee.Any fractional part of the fare is absorbed by your friendly driverA surcharge of 500 cents if a ride request is issued between 600 hrs to 900 hrs, both inclusiveIn addition, there are two types of drivers under <em>Snatch</em>. Each can provide a subset of the services above.NormalCab drivers provide JustRide and TakeACab services.PrivateCar drivers provide JustRide and ShareARide services.A customer can issue a <em>Snatch</em> ride request, specified by the distance of the ride, the number of passengers, and the time of the request.<strong>Task</strong>You shall be given a request, followed by a list of NormalCab or PrivateCar drivers together with their corresponding waiting times. A booking is a pairing of a driver with the request. The task is to find the best booking with the lowest</td>

  </tr>

 </tbody>

</table>

fare by matching the available drivers based on the services they provide to the given request. Break ties among the same lowest fares by selecting the booking with the smaller waiting time.

This task is divided into several levels. Read through all the levels to see how the different levels are related.

<h1>Level 1</h1>

Define a Request class to handle a request comprising the distance, number of passengers, and time. Include the getDistance, getNumOfPassengers and getTime methods to return the corresponding values.

Note that the Request class serves to only store data in a meaning manner for easy retrieval later. Hence the accessor (or getter) methods are part of its responsibility.

$ javac your_java_files

$ jshell your_java_files_in_bottom-up_dependency_order

jshell&gt; new Request(20, 3, 1000)

$.. ==&gt; 20km for 3pax @ 1000hrs




jshell&gt; Request request = new Request(10, 1, 900)

request ==&gt; 10km for 1pax @ 900hrs




jshell&gt; request.getDistance()

$.. ==&gt; 10




jshell&gt; request.getNumOfPassengers()

$.. ==&gt; 1

jshell&gt; request.getTime()

$.. ==&gt; 900




jshell&gt; request request ==&gt; 10km for 1pax @ 900hrs

<h1>Level 2</h1>

Now include the JustRide and TakeACab services. Note that every service needs to implement the computeFare method that returns the fare in cents.

$ javac your_java_files

$ jshell your_java_files_in_bottom-up_dependency_order jshell&gt; new JustRide()

$.. ==&gt; JustRide




jshell&gt; new JustRide().computeFare(new Request(20, 3, 1000))

$.. ==&gt; 440




jshell&gt; new JustRide().computeFare(new Request(10, 1, 900))

$.. ==&gt; 720




jshell&gt; new TakeACab()

$.. ==&gt; TakeACab




jshell&gt; new TakeACab().computeFare(new Request(20, 3, 1000))

$.. ==&gt; 860




jshell&gt; new TakeACab().computeFare(new Request(10, 1, 900)) $.. ==&gt; 530

<h1>Level 3</h1>

Now, include a NormalCab driver who is identified by its license plate number (a string) and the passenger waiting time in minutes.

Then, add a Booking class that takes in a driver and a request. From the services that a driver provides, the best service with the lowest fare is selected.

To compare two bookings using their computed fare, we let the Booking class implement the Comparable interface that specifies a compareTo method to be implemented. (note that this is different from the Comparator interface).

class Booking implements Comparable&lt;Booking&gt; {

…

@Override

<table width="606">

 <tbody>

  <tr>

   <td width="606">$ javac your_java_files$ jshell your_java_files_in_bottom-up_dependency_order jshell&gt; Driver driver = new NormalCab(“SHA1234”, 5)driver ==&gt; SHA1234 (5 mins away) NormalCab jshell&gt; new Booking(driver, new Request(20, 3, 1000))$.. ==&gt; $4.40 using SHA1234 (5 mins away) NormalCab (JustRide) jshell&gt; new NormalCab(“SHA2345”, 10)$.. ==&gt; SHA2345 (10 mins away) NormalCab jshell&gt; new Booking(new NormalCab(“SHA2345”, 10), new Request(10, 1, 900))$.. ==&gt; $5.30 using SHA2345 (10 mins away) NormalCab (TakeACab) jshell&gt; Booking b1 = new Booking(new NormalCab(“SHA2345”, 10), new Request(10, 1, 900))b1 ==&gt; $5.30 using SHA2345 (10 mins away) NormalCab (TakeACab) jshell&gt; Booking b2 = new Booking(new NormalCab(“SHA2345”, 10), new Request(10, 1, 900))b2 ==&gt; $5.30 using SHA2345 (10 mins away) NormalCab (TakeACab) jshell&gt; Comparable&lt;Booking&gt; cmp = b1cmp ==&gt; $5.30 using SHA2345 (10 mins away) NormalCab (TakeACab) jshell&gt; b1.compareTo(b2) == 0$.. ==&gt; true jshell&gt; Booking b3 = new Booking(driver, new Request(10, 1, 900)) b3 ==&gt; $5.30 using SHA1234 (5 mins away) NormalCab (TakeACab) jshell&gt; Booking b4 = new Booking(new NormalCab(“SHA2345”, 10), new Request(10, 1, 900))b4 ==&gt; $5.30 using SHA2345 (10 mins away) NormalCab (TakeACab) jshell&gt; b3.compareTo(b4) &lt; 0$.. ==&gt; true</td>

  </tr>

 </tbody>

</table>

public int compareTo(Booking other) {

…

}

}

Note that if both fares are the same, we prefer the one with the shorter waiting time.

<h1>Level 4</h1>

Now include the ShareARide service and PrivateCar driver.

You should aim to make the Booking class general such that it does not need to check for any invalid pairing, say between PrivateCar driver and TakeACab service. If you have designed your program appropriately, then extending your program with additional drivers and services would not require any modification to existing classes.

$ javac your_java_files

$ jshell your_java_files_in_bottom-up_dependency_order

jshell&gt; new ShareARide()

$.. ==&gt; ShareARide




jshell&gt; new PrivateCar(“SMA7890”, 5)

$.. ==&gt; SMA7890 (5 mins away) PrivateCar




jshell&gt; new Booking(new PrivateCar(“SMA7890”, 5), new Request(20, 3, 1000))

$.. ==&gt; $3.33 using SMA7890 (5 mins away) PrivateCar (ShareARide)




jshell&gt; new PrivateCar(“SLA5678”, 10)

$.. ==&gt; SLA5678 (10 mins away) PrivateCar




jshell&gt; new Booking(new PrivateCar(“SLA5678”, 10), new Request(10, 1, 900))

$.. ==&gt; $7.20 using SLA5678 (10 mins away) PrivateCar (JustRide)




jshell&gt; Booking b1 = new Booking(new PrivateCar(“SMA7890”, 5), new Request(10, 1, 900))

b1 ==&gt; $7.20 using SMA7890 (5 mins away) PrivateCar (JustRide)




jshell&gt; Booking b2 = new Booking(new PrivateCar(“SLA5678”, 10), new Request(10, 1, 900)) b2 ==&gt; $7.20 using SLA5678 (10 mins away) PrivateCar (JustRide)




<table width="680">

 <tbody>

  <tr>

   <td rowspan="2" width="37"> </td>

   <td width="607">jshell&gt; b1.compareTo(b2) &lt; 0 $.. ==&gt; true</td>

   <td rowspan="2" width="37"> </td>

  </tr>

  <tr>

   <td width="607"><strong>Level 5</strong>Now complete the task by defining the findBestBooking method in level5.jsh that outputs the list of matches sorted by increasing fare (breaking ties by waiting time).You may ssume that no two bookings have the same fare and the same waiting time.

    <table width="606">

     <tbody>

      <tr>

       <td width="606">jshell&gt; /open level5.jsh jshell&gt; findBestBooking(new Request(20, 3, 1000),…&gt; List.of(new NormalCab(“SHA1234”, 5), new PrivateCar(“SMA7890”, 10)))$3.33 using SMA7890 (10 mins away) PrivateCar (ShareARide)$4.40 using SHA1234 (5 mins away) NormalCab (JustRide)$4.40 using SMA7890 (10 mins away) PrivateCar (JustRide)$8.60 using SHA1234 (5 mins away) NormalCab (TakeACab) jshell&gt; findBestBooking(new Request(10, 1, 900),…&gt; List.of(new NormalCab(“SHA1234”, 5), new PrivateCar(“SMA7890”, 10)))$5.30 using SHA1234 (5 mins away) NormalCab (TakeACab)$7.20 using SHA1234 (5 mins away) NormalCab (JustRide)$7.20 using SMA7890 (10 mins away) PrivateCar (JustRide)$10.00 using SMA7890 (10 mins away) PrivateCar (ShareARide)</td>

      </tr>

     </tbody>

    </table></td>

  </tr>

 </tbody>

</table>





