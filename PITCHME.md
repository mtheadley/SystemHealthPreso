---
## Is Your application healthy?
###### This one trick will ease all of your support pains.

<img src="./assets/trick.jpg" style="border: 0px; width:350px" />

### Mark Headley

---
# Warning...

There isn't any code in this preso.

---
## ...and fair warning

<img src="./assets/smart_ass.jpg" style="border: 0px; width:350px" />

---
# Scenario

---
## Application is down....

<img src="./assets/haironfire.gif" style="border: 0px; width:350px" />

---
### User calls Tier One support.

<img src="./assets/angry-phone-user2.jpg" style="border: 0px; width:350px"/>

---
### Tier One hands it over to Tier Two (BA/PM) 

<img src="./assets/throw_over_the_wall.png" style="border: 0px; width:350px"/>

---
### Tier Two ... find a developer

<img src="./assets/mob1.jpg" style="border: 0px; width:350px"/>

---
# Triage 
---
## What went wrong?

- New code having problems? |
- What errors are the user receiving? |
- Is there anything in the error emails? |
- Anything in the logs? |
- What about the systems themselves (machines, dns, network, solar alignment, etc...) |

---
#Ah-ha!

Elastic is causing the outage... 

<img src="./assets/elastic.png" style="border: 0px; width:500px" />

---
##What to do?

- Not much you personally can do about this... | 
- You need to contact DevOps, DOC, NOC, DOC OCK, SPOCK... |

---
#What If???

- Tier One could have determined this issue when they received the call, then notified the correct people?  |

---
##...and you could keep coding?

<img src="./assets/happy_programmer.jpg" style="border: 0px; width:500px"/>

---
#Imma programmer!

<img src="./assets/how-much-money-the-characters-in-hbos-silicon-valley-would-make-in-real-life.jpg" style="border: 0px; width:500px"/>

---
##Goals

- Need to determine, at a glance, that the external systems that talks to app are up and running? |
- The result must be simple to understand. (Yes/No) |
- Result should give a simple breakdown of the systems involved. |

---
##What to do?

- Determine a way to check each external system within the application to see if it is responding. |
- Aggregate that information to show whether or not the application is 'healthy' |

---
##Easy, right?

- Let's go to a pratical example... |
- I am partially biased, so we will use EngageTV... |

--- 
#Thinking
---
##...okay, okay..."Analysis"

<img src="./assets/mini_me.gif" style="border: 0px; width:500px" />

---
##How are the external systems in ETV organized?

- Mongo |
- Elastic |
- Updaters |
	- Titlewave |
	- Harbor |
	- ASM |
	- DataCity |
	- Spotlight |

---
# How do I get what I want? 
--- 
##Mongo

<img src="assets/mongo.png" style="border: 0px; width:500px"/>

- A raw command can be executed against Mongo to determine the state of the system based on the results. |

---
##Elastic Search

<img src="assets/elastic-logo.png" style="border: 0px; width:500px"/>

- Similar to Mongo, there is a call on the Elastic API drivers that can be used.  It returns info on the state of the cluster.  We can use that info to determine the state of the system. |

---
##Updaters (EngageTVs worker process)

<img src="assets/windows_service.png" style="border: 0px; width:300px"/>

- The Updater process has a monitoring page.  If that page is responding, then the Updater service is running. |

---
##Harbor

<img src="assets/harbor.png" style="border: 0px; width:500px"/>

- Harbor has a Ping method. -- One and done. |

---
##DataCity

<img src="assets/data_city.png" style="border: 0px; width:500px"/>

- DataCity has a ping method, but it returns html.  If a 200 response is returned, then assume the site is up. |

---
##Titlewave

<img src="assets/titlewave.png" style="border: 0px; width:500px"/>

- Titlewave doesn't have a ping method...yet. |
- I can perform a GET that returns no data.  If Titlewave returns back without exception, we can assume the system is up. |

---
##Application Security Manager (ASM)

<img src="assets/asm.png" style="border: 0px; width:500px"/>

- ASM doesn't have a ping method, and probably won't.  Follow Titlewave's example. |

---
##Spotlight

<img src="assets/spotlight.png" style="border: 0px; width:500px"/>

- Use the Titlewave thinking...rinse, repeat. |
*This system will be sunsetted shortly.*

---
##Putting the pieces together

Simple JSON object
```
	SystemName
	Url
	IsAlive
	Message (used for exceptions)
```

- Put together an API endpoint |
- Put together a pretty UI |

---

##Time to build

<img src="https://media.giphy.com/media/XW3Q6lR8d7Nss/giphy.gif" style="border: 0px; width:500px"/>

---
#Demo
---
##Bonus

Other *Upstream* environments can use the endpoint in their System checks to see if the application is running correctly.

---
##Bonus #2

Since the API endpoint will return a 503 (System Unavailable) if there is an issue, and a 200 otherwise; DevOps can use the endpoint in their build process to ensure the application was deployed successfully.


<img src="assets/Commodus.jpeg" style="border: 0px; width:500px"/>

---
#Final thoughts

---
## PROS:
- Gives us a quick status of external systems used within our app.  |
- Putting this into a UI, at a glace it can be determined if there is an issue and where that issue lies. |
- Goal is to cut down on the amount of time it takes to triage why the system is down. |
- Streamline a process for DevOps' CI process. |

---
## CONS:
- This is not made for frequent polling.  Since we are cheating with a couple of the checks to external systems, it is unknown what type of effect calling the external endpoints may impact that system's resources. |
- Some of the calls may take a few moments to return. |

---
##Lingering Thoughts

- Can it be improved?  You betcha!

---
# Questions?

<img src="assets/fox_trot.jpg" style="border: 0px; width:500px"/>
