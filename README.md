# Pergunta9
--Create table tcall
create table tcall(
                oid number primary key,
                tipo number,
                subtipo number,
                data_criacao date
); 
--Create table om_record
create table om_record(
                oid number primary key,
                tipo number,
                subtipo number,
                natureza number,
                data_criacao date
);
--Create table om_record_natureza
create table om_record_natureza(
                oid number primary key,
                tipo number,
                subtipo number,
                natureza number
);
-- Inserir na tabela dados
insert into om_record_natureza values (1, 1, 1, 2);
insert into om_record_natureza values (2, 1, 2, 4);
insert into om_record_natureza values (3, 2, 1, 6);

select *
from om_record_natureza;

-- Create tigger tr_call que é ativado ao inserir dados na referida tabela e faz uma cópia para a tabela om_record
create trigger tr_tcallInsertado
on tcall for insert 
as 
Declare @oid = oid from inserted 
        @tipo = oid from inserted 
        @subtipo = oid from inserted 

insert into om_record (oid,tipo,subtipo,natureza number,data_criacao) values
(@oid,@tipo,@subtipo,'  ',getdate( ));
go;

--Create Proedure para inserir dados na tabela tcall
create Procedure SP_Inserir
AS 
  BEGIN
  insert into tcall values (1, 1, 2, 'getdate');
  insert into tcall values (3, 2, 2, 'getdate');
  insert into tcall values (4, 1, 6, 'getdate');
END;
 --fazer validacion do campo  natureza mais update 
  BEGIN 
  @tipo number,
  @subtipo number,
  @natureza number
  
  IF (@tipo = om_record_natureza.tipo) AND (@tipo = om_record_natureza.subtipo)
  ( @natureza = om_record_natureza.natureza)
  ELSE 
  (
    @natureza := 0
    );
  
  UPDATE TABLE om_record
  SET natureza =  @natureza;
  
  END;

  SELECT *
  FROM tcall T, om_record r, om_record_natureza rn
  where t.oid = r.oid
  and   r.oid = rn.oid;
