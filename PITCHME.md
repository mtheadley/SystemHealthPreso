---

## Is Your application healthy?
###### This one trick will ease all of your support pains.

![Trick](assets/trick.jpg)

### Mark Headley 


---

## Warning...

There isn't any code in this preso.

---

## Scenario

---

## ENGAGE TV is down....

<img src="./assets/haironfire.gif" width="350" />

+++

User calls Tier One support.

<img src="./assets/angry-phone-user2.jpg" width="350" />

+++

Tier One hands it over to Tier Two (BA/PM) 

<img src="./assets/throw_over_the_wall.png" width="350" />

+++

Tier Two ... hunt down a developer

<img src="./assets/mob1.jpg" width="350" />

---

## Triage 

+++

## What went wrong?

- New code having problems? |
- What errors are the user receiving? |
- Is there anything in the error emails? |
- Anything in the logs? |
- What about the system itself (machines, dns, network, solar alignment, etc...) |

+++

##Ah-ha!

Elastic is causing the outage... 

<img src="./assets/elastic.png" width="500" />

+++

##Result

- Not much you personally can do about this... | 
- You need to contact DevOps, DOC, NOC, DOC OCK, SPOCK...

---

##What If???

- Wouldn't it be great if Tier One could have detected this, notified the correct people and you could keep coding? |

---

### Imma programmer!

I can fix it!

<img src="./assets/how-much-money-the-characters-in-hbos-silicon-valley-would-make-in-real-life.jpg" width="500" />

+++

### How to Accomplish?

- How can we figure out, at a glance, that the external systems that talks to app are up and running? |

+++

### Possible Solution(s)

Create a status page / sanity check / health monitor / status endpoint

+++

### Overall Goals

- The result must be simple to understand. (Yes/No) |
- Result should give a simple breakdown of the systems involved. |

+++

### How to Implement?

- Determine a way to check each external system within the application to see if it is responding. |
- Aggregate that information to show whether or not the application is 'healthy' |

+++

### Easy, right?

- Let's go back to our pratical example... |
- I am partially biased, so we will use EngageTV... |

--- 

### Thinking...

How are the external systems in ETV organized?

- Mongo |
- Elastic |
- Updaters |
	- Titlewave |
	- Harbor |
	- ASM |
	- DataCity |
	- Spotlight |

+++

### How do I get what I want? 
##### Mongo

- A raw command can be executed against Mongo and determine based on the results if things are healthy. |

+++

##### Elastic Search

- There is an API call on the Elastic drivers that can be used.  Since the server itself is up all the time, we want to see the health of the indexes. |

+++

##### Updaters (EngageTVs worker process)

- The Updater process has a monitoring page.  If that page is responding, then the Updater service is running. |

+++

##### Harbor

- Harbor has a Ping method. -- One and done. |

+++

##### DataCity

- DataCity has a ping method, but it returns html.  If a 200 response is returned, then assume the site is up. |

+++

##### Titlewave

- Titlewave doesn't have a ping method...yet. |
- *BUT* I can perform a GET that returns no data.  If Titlewave returns back without exception, we can assume the system is up. |

+++

##### ASM

- ASM doesn't have a ping method, and probably won't.  Follow Titlewave's example. |

+++

##### Spotlight

- Use the Titlewave thinking...rinse, repeat. |
*This system will be sunsetted shortly.*

---

### Putting the pieces together

- Simple JSON object |
```
	SystemName
	Url
	IsAlive
	Message (used for exceptions)
```

- Put together an API endpoint |
- Put together a pretty UI |

+++

### Time to build

<img src="https://media.giphy.com/media/XW3Q6lR8d7Nss/giphy.gif" width="500"/>

+++

### Demo

---

###Bonus

- Since the API endpoint will return a 503 (System Unavailable) if there is an issue, and a 200 otherwise; DevOps can use the endpoint in their build process to ensure the application was deployed successfully. |


+++

###Bonus #2

- Other *Upstream* environments can use the endpoint in their System checks to see if EngageTV is running correctly. |

---

###Final thoughts

PROS:
- Gives us a heads up on the systems that ETV talks with.  We are able to, at a glace, determine if there is an issue and where that issue lies. |
- Goal is to cut down on the amount of time it takes to triage why the system is down. |
- Streamline a process for DevOps' CI process. |

+++

CONS:
- The System health mechanism is not made for frequent polling.  Since we are cheating with a couple of the checks to external systems, it is unknown what type of effect calling the external endpoints may impact that system's resources. |
- Some of the calls make take a few moments to return. |

+++

###Lingering Thoughts

- Can it be improved?  You betcha!

---

### Questions?








