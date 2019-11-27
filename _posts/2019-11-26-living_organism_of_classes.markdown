---
layout: post
title:      "Living organism of Classes"
date:       2019-11-27 03:07:04 +0000
permalink:  living_organism_of_classes
---


Object Oriented Programming is all about creation, functioning, connection and putting into action the separate classes interchangeably. This is one of the key concepts in programming which is initially hard to grasp, but pays off and facilitates creation of logically associated classes later on. 
There are many real life examples and metaphors, explaining class relationship in OOP, both preparing programmers to the real world situations, and making the concept easier to digest. As one of such example we can consider doctor-patients association (note that doctor is singular, but patients aren't). Curing a disease is a challenge to meet, and we need to handle it somehow. We can say, that **Doctor has many patients** and **Patients belong to Doctor**. Let's write some code to visualize our treatment plan. So, for instance, we have two separate classes (in two separate .rb files)

```
class Doctor

attr_accessor :name

    @@all = []
		
    def initialize (name)
       @name = name
  		 @@all << self
	   	 @patients = []
    end
		
	def add_patient (patient)
	    @patients << patient
	    patient.doctor = self
	end
		
	def patients
    @patients.each.with_index(1) do |patient, index| 
        puts "#{index}. #{patient.name}, #{patient.age}, #{patient.diagnose}"
    end
  end
		
	def self.print_doctor_names
	    @@all.each do |doctor|
	    puts "#{doctor.name}"
	end
	end
end

adam = Doctor.new("Adam")
john = Doctor.new("John")


class Patient

attr_accessor :name, :age, :diagnose, :doctor

@@all = []

    def initialize (name, age, diagnose)
          @name = name
					@age = age
					@diagnose = diagnose
					@@all << self
    end
end

samantha = Patient.new("Samantha", 53, "CAD")
derek = Patient.new("Derek", 65, "type 2 diabetes")
deborah = Patient.new("Deborah", 57, "metabolic syndrome")
steven = Patient.new("Steven", 45, "dyslipidemia")
```

How do we connect them interchangeably? Well, it's easier than you think. First of all, we need to tell the patients who their doctor is. So, in `attr_accessor` of `class Patient` we added `:doctor ` as you might have noticed.
When we call `Doctor.all`, it will return us both `adam` and `john` instances, because they now live in the class variable of `Doctor.all`. The same situation stands with `Patient.all`, which will return all the patients ever registered in the hospital. But what if we need to know all the patients of one specific doctor, like Dr. Adam?
Each time a new patient gets ill, he/she will be saved (or pushed, or shoveled, you pick) in `Patient.all`, and we can name those patients in the following manner (taking into consideration that each time a new instance of patient calls for medical aid, it must have a name, age and diagnosis). The `initialize` method is common for all the classes, which says what arguments should be taken each time a new instance of a class is created, for example, we can assume that all the patients have name, age, and illness upon creation of an instance, or initialization. Should we have less arguments, arguments error will come out, pointing out that we need to pass the same amount of arguments in instance creation as in `initialize` method.
We can also tell the patients who their doctor is. For example, let's assume that Dr. Adam has two patients, Samantha and Derek, and Dr. John also has two patients, Deborah and Steven. We can make this connection by calling `add_patient` instance method for every doctor, which means, that each doctor now can have his own list of patients within common Doctor class, as shown in the example below:

```
adam.add_patient(samantha)
adam.add_patient(derek)
john.add_patient(deborah)
john.add_patient(steven)
```

In this case, we can say, that doctor **has many** patients, and patients **belong to** one doctor. 

From now on, we can know for sure how many patients Dr. Adam has, along with other vital information, like age and diagnosis, which will help our doctor choose right treatment option for his patients. By simply calling `adam.patients` we can now iterate over all patients instances belonging to that particular doctor instance and get all information we require. The same is true for `john.patients`.
Moreover, we can double check this connection by going in opposite direction. Let’s pretend both `samantha` and `derek` want to know who their doctor is. As intuitive as it can get, we can simply say `samantha.doctor.name` and `derek.doctor.name`, which will immediately print out the same “Adam”.
With all the secrets revealed, we can now say that both our individual classes, being two different types of building blocks and having their own behavior with all the unique class and instance methods, are connected and can change information, like body parts of one organism, which is why Object Oriented Programming and Object Relationship is awesome!
