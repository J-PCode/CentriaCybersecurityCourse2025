# CS – Phase 3 Authorization Test Report

## Environment
- Application: Booking System (Phase 3)
- URL: http://localhost:8003
- Testing method: Browser testing, OWASP ZAP, endpoint discovery
- Date: 16.12.2025
- Tester: J-PCode

---

## Role: Guest (Not logged in)

### Can do
- Access landing page `/`
- View existing reservations/booked resources without seeing reserver identity
- Access login page `/login`
- Access registration page `/register`
	- Registrate as new reserver
	- register as new administrator
### Cannot do
- Access reservation creation page `/reservation` (redirected to login)
- Submit reservation requests
- Create new resources
- Access admin pages `/admin/*`
- Modify or delete any resources

---

## Role: Reserver (Authenticated user)

### Can do
- Log in to the system `/login`
- Create new resource `/resources` ⚠️ (Unexpected – potential broken access control)
- Create a reservation for a resource (after adding manually timefields) `/reservation
	- change username, 
	- select resource,
	- change start and end time
- View booked reservations `/
- View public resource list `/`
- edit own reservations `/reservation?id=*`
	- Update
	- Delete
	- Cancel
### Cannot do
- Access admin dashboard `/admin`
- Modify or delete resources ⚠️ (Spec violation if allowed)
- View or modify other users’ reservations
- view or modify user information

---

## Role: Administrator (Admin user)

### Can do
- Log in as administrator
- Access admin dashboard `/admin`
- Add new resources
- Modify existing resources
- Delete resources
- View all reservations
- Log in to the system `/login`
- Create new resource `/resources`
- Create a reservation for a resource (after adding manually timefields) `/reservation
	- change username, 
	- select resource,
	- change start and end time
- View booked reservations `/`
- View public resource list `/`
- edit own reservations `/reservation?id=*`
	- Update
	- Delete
	- Cancel
- Edit/remove other users booked resources
- Edit/remove added Resources `/resources?id=*`

### Cannot do
- Modify user account details
- View user personal information
- Delete user accounts (reservers)

---

## Observations
- Administrator does not have access to user account management.
	- This limits exposure of personal data and aligns with the principle of least privilege and GDPR considerations.
	- However, this deviates from the specification, which states that the administrator can delete reservers.
- Authorization checks are enforced at the backend.
- Unauthorized access attempts return redirects or HTTP 403 responses.
- No privilege escalation was observed during browser testing.

---

## Tools Used
- Web browser (manual testing)
- OWASP ZAP (authenticated and unauthenticated scans)
- OWASP ZAP (spidering and passive scan)

---

## Conclusion
Authorization controls were tested for all defined roles.  
Access restrictions generally follow the specification, and no critical authorization bypasses were observed during testing.

---
## Endpoint Discovery Summary

Endpoint discovery was performed using:
- Manual browser exploration
- OWASP ZAP spidering and passive scan
- robots.txt and sitemap.xml analysis

No additional hidden or unreferenced endpoints were discovered beyond those already documented.

---
## Conclusion
Authorization controls were tested for all defined roles.  
Access restrictions generally follow the specification, and no critical authorization bypasses were observed during testing.

---