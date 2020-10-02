# comanage-registry-plugin-CoMfaService

COmanage 3.1.1 plugin that fetches the users mobile phone numbers and allows validation

## Installation

1. Run `git clone https://github.com/rciam/comanage-registry-plugin-CoMfaService.git /path/to/comanage/local/Plugin/CoMfaService`
2. Run `cd /path/to/comanage/app`
3. Run `Console/clearcache`
4. Run `Console/cake schema create --file schema.php --path /path/to/comanage/local/Plugin/CoMfaService/Config/Schema`

## Schema update
Not yet implemented

## Configuration
- Need to make the mobile phone number attribute editable by the user. This is an admin action only
```
Co -> Configuration -> Self Service Permissions
```
 | Key       | Value            |
 |-----------|------------------|
 |Model      | Telephone Number |
 |Type       | Mobile           |
 |Permission | Read Write       |

- Add a new table in the database
```
// Add the missing constraints
ALTER TABLE ONLY public.cm_co_mfa_service_settings ADD CONSTRAINT cm_co_mfa_service_settings_co_id_fkey FOREIGN KEY (co_id) REFERENCES public.cm_cos(id);
GRANT SELECT ON TABLE public.cm_co_mfa_service_settings TO cmregistryuser_proxy;

ALTER TABLE ONLY public.cm_co_mfa_services ADD CONSTRAINT cm_co_mfa_services_co_id_fkey FOREIGN KEY (co_id) REFERENCES public.cm_cos(id);
ALTER TABLE ONLY public.cm_co_mfa_services ADD CONSTRAINT cm_co_mfa_services_co_person_id_fkey FOREIGN KEY (co_person_id) REFERENCES public.cm_co_people(id);
ALTER TABLE ONLY public.cm_co_mfa_services ADD CONSTRAINT cm_co_mfa_services_telephone_number_id_fkey FOREIGN KEY (telephone_number_id) REFERENCES public.cm_telephone_numbers(id);
ALTER TABLE ONLY public.cm_co_mfa_services ADD CONSTRAINT cm_co_mfa_services_co_mfa_service_setting_id_fkey FOREIGN KEY (co_mfa_service_setting_id) REFERENCES public.cm_co_mfa_service_settings(id);
GRANT SELECT ON TABLE public.cm_co_mfa_services TO cmregistryuser_proxy;
```
