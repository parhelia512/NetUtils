��� ��������� (�������� ������ � trayicon ����������) ���� ��������
� ����� ����� ���������� ��������� TCP ����������� � InterBase (��� � ������ ������� TCP) �������.
��� ���������� ���������� (IP Address, Host Name (���� resolve �������),
DateTime ����� � ������) � interbase.log (��� ����� ������ ��������� ����),
� ���������� ���������� � �������� ������������ �� ������.
����� ����� ������������� ��������� �������� �����������.

�������� - ���� �� ������ ���������� ������, ���������� ������� �.

������:

1.����� ���������� ������ ��������� � ��������� ������ ipconsvc.exe /install.
2.��������� "Services", ������� ������ "IP Connections" �
  ��������� ���, � "Startup" ���������� ��������� ��� ������ "Manual" ��� "Automatic".
3.��� �������� �� ������� ��������� � ��������� ������ ipconsvc.exe /uninstall.

����������:

� ����������� �� ���������, ������ ���������. 

��������� ����������: Windows XP � ����, InterBase Server 4x � ����, ��� ����� ����, ��� ����� ������ TCP ������.

� ���������� ���������������� ���� IPConSvc.cfg, (�� ������ ��������� � ��� �� �����
��� � ���������) ������� �������� ��������� �� ��������� ��� Firebird 3. 

ServerPort=3050 - ���� �������
LogFile=C:\Program Files (x86)\Firebird\Firebird_3_0\firebird.log - ������ ���� � log �����
TimeOut=1000 - ����� ������� � ������������� (�� 0 � ����) 
SingleLine=False - ��. ����
ClientPort=11970 - TCP ���� ��� ipconclt.exe

���� �������� SingleLine=False (��� ������ � interbase.log) c����� � ����� ����� ��������� ���:

SERVER	Sun Nov 27 17:05:05 2016
	IP Connections/Connect: address = 167.33.33.28, hostname - HOST1


SERVER	Sun Nov 27 17:05:09 2016
	IP Connections/Disconnect: address = 167.33.33.28, hostname - HOST1

���� �������� SingleLine=True (��� ������ � ����� ������ ��������� ����) c����� � ����� ����� ��������� ���:

167.33.33.28    HOST1    11/27/2016 17:05:05    11/27/2016 17:05:09

�� ������ �������� ������� � ����� �� ��� ������ �� ������� �������:

  CREATE TABLE IP_CONNECT EXTERNAL FILE '������ ���� � log �����'(
    IP_ADDRESS CHAR(15),
    HOST_NAME CHAR(255),
    DT_OPEN CHAR(19),
    DT_CLOSE CHAR(19),
    EOL CHAR(2)
  )

� �������� � ���.

������� ��������:

���� �� ������ ���������� ����������� �� ���� ���� ��������� ��������� ������:

  select * from ip_connect
  where cast(dt_open as date) between '11/26/2016' and '11/27/2016'

���� �� ������ ���������� ����������� �� IP ������ ��������� ��������� ������:

  select * from ip_connect
  where cast(ip_address as varchar(15)) = '167.33.33.28'

���� �� ������ ���������� ����������� �� ����� ����� ��������� ��������� ������:

  select * from ip_connect
  where cast(host_name as varchar(255)) = 'HOST1'
