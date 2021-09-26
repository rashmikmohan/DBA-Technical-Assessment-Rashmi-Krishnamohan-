# DBA-Technical-Assessment-Rashmi-Krishnamohan-
F8th Inc Technical Interview

MySQL commands for technical interview:

%Created tabke for country_name as primary key that references to the country column in dbatest.citizen table 

CREATE TABLE `dbatest`.`country` (
  `id` INT NOT NULL,
  `country_name` VARCHAR(45) NULL,
  `count` INT NULL DEFAULT 0,
  PRIMARY KEY (`id`));
  
  %add foreign key constraint to column country in dbatest.citizen 
  
  ALTER TABLE `dbatest`.`citizen` 
ADD CONSTRAINT `FK_Country`
  FOREIGN KEY (`country`)
  REFERENCES `dbatest`.`country` (`id`)
  ON DELETE NO ACTION
  ON UPDATE NO ACTION;
  
  
  % trigger count of dbatest.country to increment after any insert occurs
  
  DROP TRIGGER IF EXISTS `dbatest`.`citizen_AFTER_INSERT`;

DELIMITER $$
USE `dbatest`$$
CREATE DEFINER = CURRENT_USER TRIGGER `dbatest`.`citizen_AFTER_INSERT` AFTER INSERT ON `citizen` FOR EACH ROW
BEGIN
	UPDATE dbatest.COUNTRY SET count = count +1  where id=new.country;
END$$
DELIMITER ;


%store proc to provide citizen details of country that is called and reset the count to 0.

CREATE PROCEDURE `Get_CitizenName` ( IN countryName varchar(25))
BEGIN
select * from dbatest.citizen where country IN 
(Select id from dbatest.country where lower(country_name) = lower(countryName));
update dbatest.country set count=0 where lower(country_name) = lower(countryName);
END
