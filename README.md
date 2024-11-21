# Challenge-Submission
import random
from datetime import datetime, timedelta

# Mock Citizen Data (This would be from a government database)
citizens = [
    {"citizen_id": 1, "name": "Alice", "age": 28, "health_conditions": ["None"], "last_taxes_filed": datetime(2022, 4, 15), "vaccination_status": "Up to date"},
    {"citizen_id": 2, "name": "Bob", "age": 60, "health_conditions": ["Diabetes"], "last_taxes_filed": datetime(2022, 4, 1), "vaccination_status": "Not up to date"},
    {"citizen_id": 3, "name": "Charlie", "age": 35, "health_conditions": ["Asthma"], "last_taxes_filed": datetime(2023, 2, 10), "vaccination_status": "Up to date"},
    # Add more citizen data as needed
]

# Example services available
available_services = [
    {"service_id": 1, "service_name": "Tax Filing Reminder", "service_type": "Tax", "age_group": "All", "health_condition": "None", "frequency": "Annually"},
    {"service_id": 2, "service_name": "Health Checkup", "service_type": "Health", "age_group": "60+", "health_condition": "Diabetes", "frequency": "Annually"},
    {"service_id": 3, "service_name": "Vaccination Reminder", "service_type": "Health", "age_group": "All", "health_condition": "None", "frequency": "Every 2 Years"},
    {"service_id": 4, "service_name": "Renew Driver's License", "service_type": "Admin", "age_group": "18+", "health_condition": "None", "frequency": "Every 5 Years"},
    # More services can be added
]
def recommend_services(citizen, services):
    recommendations = []

    # Check age group
    if citizen["age"] >= 60:
        recommendations.append({"service_name": "Health Checkup", "reason": "Age 60+"})
    
    # Check health condition
    if "Diabetes" in citizen["health_conditions"]:
        recommendations.append({"service_name": "Health Checkup", "reason": "Diabetes"})

    # Check if they need to file taxes
    last_tax_date = citizen["last_taxes_filed"]
    if (datetime.now() - last_tax_date).days > 365:  # If it's been more than a year
        recommendations.append({"service_name": "Tax Filing Reminder", "reason": "File taxes"})

    # Check if vaccination status is up-to-date
    if citizen["vaccination_status"] == "Not up to date":
        recommendations.append({"service_name": "Vaccination Reminder", "reason": "Vaccination overdue"})

    # Add generic recommendations based on the profile
    for service in services:
        if service["age_group"] == "All" and service["health_condition"] == "None":
            recommendations.append({"service_name": service["service_name"], "reason": "General service"})

    return recommendations

# Example: Recommend services for each citizen
for citizen in citizens:
    print(f"\nRecommendations for {citizen['name']}:")
    services_for_citizen = recommend_services(citizen, available_services)
    if services_for_citizen:
        for service in services_for_citizen:
            print(f" - {service['service_name']} ({service['reason']})")
    else:
        print(" - No recommendations at this time.")
def send_notification(citizen, service):
    # This is a simplified notification function.
    # In reality, you'd integrate with an email/SMS system.
    print(f"Sending notification to {citizen['name']} for: {service['service_name']} ({service['reason']})")

# Example of sending proactive notifications
for citizen in citizens:
    services_for_citizen = recommend_services(citizen, available_services)
    for service in services_for_citizen:
        send_notification(citizen, service)

