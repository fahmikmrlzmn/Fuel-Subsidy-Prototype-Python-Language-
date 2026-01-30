# Fuel-Subsidy-Prototype-Python-Language-
Eligibility checking and provide fraud detection 

This prototype includes a simple citizen eligibility checker that simulates how the Malaysian government verifies whether a user qualifies for the fuel subsidy (e.g., Budi Madani / BUDI95).

MyKad Eligibility Check
The system uses a simple simulated MyKad database:

citizens = { "900101081234": {"name": "Ali", "eligible": True}, "850202061111": {"name": "Siti", "eligible": False}, }

Vehicle Registration Check
The app also includes a simple vehicle database:

registered_vehicles = { "ABC1234": {"owner": "900101081234"}, "BXY9081": {"owner": "850202061111"}, }
