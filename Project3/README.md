<h1>A Query Execution Engine</h1>
<hr/>
<p>
Cory Hayward, He Xuebin, Thinh Nguyen <br/>
Project 3 for CS542: Database Management Systems <br/>
Professor Singh <br/>
1 April 2015
</p>

Please note that model and class in this report will be used interchangeably.

<h3> Introduction </h3>
This project aimed to build a query execution engine by implementing the following requirement.
<blockquote>
"
You may have realized that the functions of exercise 1 are actually methods of a Relation class. In a real database, we will have multiple instances of Relation, each representing one table. Modify your answer from exercise 1 to create the class and also add open(), getNext() and close() methods to it. Use the class to create and populate city and country tables (schema and data to be provided during January). Use the class to find all cities whose population is more than 40% of the population of their entire country.
"
</blockquote>

<h3> Record </h3>
<p>
	The record model or class is a way that we abstractly represent data. It has a title row and multiple data rows. This class has two crucial methods those are select and union.
	
	void select(String[] names); //choose attributes to present
	void union(Record r); //union this record with record of interest

</p>
<h3> Relation </h3>
<p>
	Relation class represent a relation by holding a pointer to the relation csv file and implementing open(), getNext() and close() methods
</p>

<p>
	open() : everytime a relation call open(), it will load the next block of data into the memory <br/>
	getNext() : get the next row of the relation <br/>
	close() : close the process <br/>
</p>
<h3> Condition </h3>
<p>
The condition class take care of the condition of the join. It keeps track of three important operations: equal, smaller, and greater. For example, if attr a is less than attr b, the condition will compare values at each tuple at particular attributes.
<p>
<h3> Join Operation</h3>
<p>
The join class aims to represent the join operation. It takes in two relation objects and multiple conditions for the join. The getNext() method will consider all the conditions between two relations (for every record in the left relation, check condition for all record in the right)
</p>
<h3> Join Mechanism</h3>
<pre>
open { <br/>
	left.open() <br/>
	right.open() <br/>
} <br/>


getNext { <br/>
	if (right == null) {<br/>
		left.getNext() <br/>
		if (left.getNext() is null);<br/>
			close()<br/>
		right.close()<br/>
		right.open()<br/>
	}<br/>
	
	if (t in Right match t in Left) {<br/>
		build the union and output<br/>
	}<br/>
	
}<br/>

close {<br/>
	left.close()<br/>
	right.close()<br/>
}
</pre>

<h3> Assumption </h3>
- Relation class heavily depends on FileInputStream and is built for file reading
- Assumed that both relation's size are not too different from each other. If one relation's is significantly less than that of other relation, placing smaller size relation on the left can significantly improve performance.

<h3> Testing </h3>
<h5> Scenario 1 </h5>
<p>
<strong>City Mock Relation</strong> <br/>
Id, City, Country, CountryCode<br/>
1, Singapore, Singapore,20<br/>
2, Ho Chi Minh, Vietnam,10<br/>
3, Beijing, China,100<br/>
4, MA, US, 50<br/>
</p>

<p>
<strong>Country Mock Relation</strong> <br/>
Id, Country, Population <br/>
100, China, 100000 <br/>
10, Vietnam, 80000000 <br/>
123, Zimbawe, 30000000 <br/>
50, US, 300000000 <br/>
</p>

<p>
As we are trying to join the above two relation when city.CountryCode = country.id. We expect the result as below <br/>
<strong> Expected Result </strong> <br/>
10, Vietnam, 80000000, 2, Ho Chi Minh, Vietnam, 10 <br/>
100, China, 100000, 3, Beijing, China, 100
</p>

<h5> Scenario 2 </h5>
<p> City csv file </p>
<p> Country csv file </p>
<p> 
	<strong>Expected </strong>
	40 percent of country polution is less than city's population
</p>

<h5> Additional features </h5>
<p>
One additional feature that we implemented was projection. We would like to test if this feature work by specifying which attributes we want to display. Please refer to the test folder for additional information.
</p>
