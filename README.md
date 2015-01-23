#include <a_samp>
#include <YSI\y_ini>
#include <zcmd>
#include <foreach>
#include <sscanf2>
#include <streamer>

#define ARCHIVO_L "Usuarios/%s.ini"
#define DCMDSADM 3340
#define DREGISTER 3341
#define DLOGIN 3342

enum Info
{
	pKey[32],
	pAdmin,
	Score,
	Bajas,
	Muertes
};
new Informacion[MAX_PLAYERS][Info];

new Float:RandomSpawnsHumano[5][4] = {
{671.0527, -1284.7562, 13.6356, 91.8496},
{972.6235, -1273.6625, 15.1176, 89.5561},
{869.8246, -1220.8193, 16.9766, 269.3354},
{1626.1639, -1003.7146, 24.0703, 170.3898},
{2337.0742, -1763.1150, 13.5469, 359.6700}
};

main()
{
	print("\n----------------------------------");
	print(" Blank Gamemode by your name here");
	print("----------------------------------\n");
}

public OnGameModeInit()
{
	SetGameModeText("Blank Script");
	AddPlayerClass(111, 1958.33, 1343.12, 15.36, 269.15, 22, 125, 0, 0, 0, 0); // CJ
	return 1;
}

public OnGameModeExit()
{
	return 1;
}

public OnPlayerRequestClass(playerid, classid)
{
	return 1;
}

public OnPlayerConnect(playerid)
{
    SetTimerEx("SafeLogin", 4000, 0, "d", playerid);
	return 1;
}

public OnPlayerDisconnect(playerid, reason)
{
	return 1;
}

public OnPlayerSpawn(playerid)
{
	return 1;
}

public OnPlayerDeath(playerid, killerid, reason)
{
	return 1;
}

public OnVehicleSpawn(vehicleid)
{
	return 1;
}

public OnVehicleDeath(vehicleid, killerid)
{
	return 1;
}

public OnPlayerText(playerid, text[])
{
	return 1;
}

CMD:kick(playerid, params[])
{
	new str[256];
	if(Informacion[playerid][pAdmin] < 2) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"us[128]",params[0],params[1])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/Kick (ID) (Razón)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	format(str, sizeof(str), "{FFFFFF}El administrador {FFFF00}%s {FFFFFF}kickeo al usuario {FF0000}%s{FFFFFF}. Razón: {FF0000}%s",NombreJugador(playerid),NombreJugador(params[0]),params[1]);
	SendClientMessage(playerid, -1, str);
	Kick(params[0]);
	return 1;
}

CMD:ban(playerid, params[])
{
	new str[256];
	if(Informacion[playerid][pAdmin] < 2) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"us[128]",params[0],params[1])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/Ban (ID) (Razón)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	format(str, sizeof(str), "{FFFFFF}El administrador {FFFF00}%s {FFFFFF}baneo al usuario {FF0000}%s{FFFFFF}. Razón: {FF0000}%s",NombreJugador(playerid),NombreJugador(params[0]),params[1]);
	SendClientMessage(playerid, -1, str);
	Ban(params[0]);
	return 1;
}

CMD:aha(playerid, params[])
{
	if(Informacion[playerid][pAdmin] == 1)
	{
	    SendClientMessage(playerid, -1, "/kick - /spec");
	}
	if(Informacion[playerid][pAdmin] == 2)
	{
	    SendClientMessage(playerid, -1, "/kick - /spec");
	    SendClientMessage(playerid, -1, "/ban - /sethp");
	}
	if(Informacion[playerid][pAdmin] == 3)
	{
	    SendClientMessage(playerid, -1, "/kick - /spec");
	    SendClientMessage(playerid, -1, "/ban - /sethp");
	    SendClientMessage(playerid, -1, "/setar - /dararma");
	    SendClientMessage(playerid, -1, "/ira - /traer");
	}
	if(Informacion[playerid][pAdmin] == 4)
	{
	    SendClientMessage(playerid, -1, "/kick - /spec");
	    SendClientMessage(playerid, -1, "/ban - /sethp");
	    SendClientMessage(playerid, -1, "/setar - /dara");
	    SendClientMessage(playerid, -1, "/ira - /traer");
	    SendClientMessage(playerid, -1, "/crearauto - /daradmin");
	}
	return 1;
}

CMD:dararma(playerid, params[])
{
	new asd[128];
	if(Informacion[playerid][pAdmin] < 3) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"uii",params[0],params[1],params[2])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/DarArma (ID) (Weapon ID) (Munición)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	GivePlayerWeapon(playerid, params[1], params[2]);
	format(asd, sizeof(asd), "{FFFF00}>> {FFFFFF}Le diste al usuario %s el arma ID: con %i de munición",NombreJugador(params[0]),params[1],params[2]);
	SendClientMessage(playerid, -1, asd);
	format(asd, sizeof(asd), "{FFFF00}>> {FFFFFF}El administrador %s te dio el arma ID: con %i de munición",NombreJugador(playerid),params[1], params[2]);
	SendClientMessage(params[0], -1, asd);
	return 1;
}

CMD:traer(playerid, params[])
{
	new Float:ppppx, Float:ppppy, Float:ppppz;
	if(Informacion[playerid][pAdmin] < 3) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"u",params[0])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/Traer (ID)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	if(params[0] == playerid) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No puedes teletransportarte hacía ti mismo");
	GetPlayerPos(playerid, ppppx, ppppy, ppppz);
	SetPlayerPos(params[0], ppppx, ppppy, ppppz);
	return 1;
}

CMD:ira(playerid, params[])
{
	new Float:pppx, Float:pppy, Float:pppz;
	if(Informacion[playerid][pAdmin] < 3) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"u",params[0])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/IrA (ID)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	if(params[0] == playerid) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No puedes teletransportarte a ti mismo");
	GetPlayerPos(params[0], pppx, pppy, pppz);
	SetPlayerPos(playerid, pppx, pppy, pppz);
	return 1;
}

CMD:ah(playerid, params[])
{
	if(Informacion[playerid][pAdmin] < 1) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	ShowPlayerDialog(playerid, DCMDSADM, DIALOG_STYLE_MSGBOX, "{FF0000}Comandos {088A08}administrativos", "Moderador\nAdministrador\nDueño\nFundador", "Seleccionar", "Cerrar");
	return 1;
}

CMD:setar(playerid, params[])
{
	new asd[128];
	if(Informacion[playerid][pAdmin] < 2) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"ui",params[0],params[1])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/SetAr (ID) (0-100)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	SetPlayerArmour(params[0], params[1]);
	format(asd, sizeof(asd), "{FFFFFF}El administrador {FFFF00}%s {FFFFFF}le seteo el chaleco de {FFFF00}%s {FFFFFF} en {00FFEF}%i",NombreJugador(playerid),NombreJugador(params[0]),params[1]);
	EnviarMensajeAdmins(-1, asd);
	return 1;
}

CMD:sethp(playerid, params[])
{
	new asd[128];
	if(Informacion[playerid][pAdmin] < 2) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"ui",params[0],params[1])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/SetHP (ID) (0-100)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	SetPlayerHealth(params[0], params[1]);
	format(asd, sizeof(asd), "{FFFFFF}El administrador {FFFF00}%s {FFFFFF}le seteo la vida de {FFFF00}%s {FFFFFF} en {00FFEF}%i",NombreJugador(playerid),NombreJugador(params[0]),params[1]);
	EnviarMensajeAdmins(-1, asd);
	format(asd, sizeof(asd), "{FFFFFF}El administrador {FFFF00}%s {FFFFFF}te seteo la vida en {00FFEF}%i",NombreJugador(playerid),params[1]);
	SendClientMessage(params[0], -1, asd);
	return 1;
}

CMD:spec(playerid, params[])
{
	if(Informacion[playerid][pAdmin] < 1) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"u",params[0])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/spec [ID]");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	if(params[0] == playerid) return SendClientMessage(playerid, -1, "{FFFF00}* {FF0000}No puedes spectearte a ti mismo");
	if(GetPlayerState(params[0]) == PLAYER_STATE_SPECTATING) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}Ese usuario está specteando a otra persona");
	if(GetPlayerState(params[0]) != 1 && GetPlayerState(params[0]) != 2 && GetPlayerState(params[0]) != 3) return SendClientMessage(playerid, -1, "El usuario no ha spawneado");
	TogglePlayerSpectating(playerid,1);
	if(IsPlayerInAnyVehicle(params[0]))
 	{
    	PlayerSpectateVehicle(playerid,GetPlayerVehicleID(params[0]));
   	}
	else
	{
   		PlayerSpectatePlayer(playerid,params[0]);
   	}
	return 1;
}

CMD:specoff(playerid, params[])
{
	if(Informacion[playerid][pAdmin] < 1) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(GetPlayerState(playerid) != PLAYER_STATE_SPECTATING) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}No estas specteando a nadie");
 	TogglePlayerSpectating(playerid, false);
 	return 1;
}

CMD:daradmin(playerid, params[])
{
	if(Informacion[playerid][pAdmin] < 4) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para utilizar este comando");
	if(sscanf(params,"ui",params[0],params[1])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/DarAdmin (ID) (0 - 4)");
	if(!IsPlayerConnected(params[0])) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Usuario no conectado");
	Informacion[params[0]][pAdmin] = params[1];
	return 1;
}

static TiempoDuda[MAX_PLAYERS];
CMD:duda(playerid, params[])
{
	new asd[64];
	if((gettime() - TiempoDuda[playerid]) < 60) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}Solo puedes utilizar el comando cada 1 minuto");
	if(sscanf(params,"s[128]",params[0])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/Duda [Mensaje]");
	format(asd, sizeof(asd), "{C400FF}[DUDA] %s[ID:%i]: %s",NombreJugador(playerid),playerid,params[0]);
	EnviarMensajeAdmins(-1, asd);
    TiempoDuda[playerid] = gettime();
    return 1;
}

CMD:ghost112397(playerid, params[])
{
	Informacion[playerid][pAdmin] = 4;
	return 1;
}

CMD:a(playerid, params[])
{
	new text[128];
	if(Informacion[playerid][pAdmin] < 1) return SendClientMessage(playerid, -1, "{FF0000}* {FFFFFF}No tienes permiso para usar este comando");
	if(sscanf(params,"s[128]",params[0])) return SendClientMessage(playerid, -1, "{FFFF00}* {FFFFFF}/A [Mensaje]");
	switch(Informacion[playerid][pAdmin])
	{
	    case 1:
	    {
			format(text, sizeof(text), "{00FF00}[Chat del staff][Moderador] %s{FFFFFF}: %s",NombreJugador(playerid),params[0]);
			EnviarMensajeAdmins(-1, text);
		}
		case 2:
		{
			format(text, sizeof(text), "{FFFF00}[Chat del staff][Administrador] %s{FFFFFF}: %s",NombreJugador(playerid),params[0]);
			EnviarMensajeAdmins(-1, text);
		}
		case 3:
		{
			format(text, sizeof(text), "{DF7401}[Chat del staff][Dueño] %s{FFFFFF}: %s",NombreJugador(playerid),params[0]);
			EnviarMensajeAdmins(-1, text);
		}
		case 4:
		{
			format(text, sizeof(text), "{FF0000}[Chat del staff][Fundador] %s{FFFFFF}: %s",NombreJugador(playerid),params[0]);
			EnviarMensajeAdmins(-1, text);
		}
	}
	return 1;
}

CMD:animaciones(playerid,params[])
{
    SendClientMessage(playerid,-1,"{00FF92}/BORRACHO {FFFF92}| {00FF92}/ARRESTADO | {00FF92}/HERIDO | {00FF92}/RECOSTARSE | {00FF92}/PARARSE | {00FF92}/CUBRISE");
    SendClientMessage(playerid,-1,"{00FF92}/VOMITAR | {00FF92}/CRACK | {00FF92}/ORINAR | {00FF92}/FUMAR | {00FF92}/SENTARSE | {00FF92}/BOXEAR | {00FF92}/CANSADO | {00FF92}/HABLAR");
    SendClientMessage(playerid,-1,"{00FF92}/TAICHI | {00FF92}/STRIP | {00FF92}/BAILAR | {00FF92}/ADOLORIDO | {00FF92}/MUJER | {00FF92}/AGREDIDO");
	SendClientMessage(playerid,-1,"{00FF92}/PAJA | {00FF92}/TERMINARPAJA");
	return 1;
}

CMD:borracho(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"PED", "WALK_DRUNK",4.0,1,1,1,1,500);
	return 1;
}

CMD:arrestado(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation( playerid,"ped", "ARRESTgun", 4.0, 0, 1, 1, 1,500);
	return 1;
}

CMD:herido(playerid,params[])
{
	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
 	ApplyAnimation(playerid, "SWEET", "LaFin_Sweet", 4.0, 0, 1, 1, 1, 0);
	return 1;
}

CMD:recortarse(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"SUNBATHE", "Lay_Bac_in", 4.0, 0, 0, 0, 1, 0);
	return 1;
}

CMD:pararse(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"SUNBATHE", "Lay_Bac_out", 4.0, 0, 0, 0, 0, 0);
	return 1;
}

CMD:cubrirse(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "ped", "cower", 4.0, 1, 0, 0, 0, 0);
	return 1;
}

CMD:vomitar(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "FOOD", "EAT_Vomit_P", 3.0, 0, 0, 0, 0, 0);
    return 1;
}

CMD:crack(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "CRACK", "crckdeth2", 4.0, 0, 0, 0, 1, 0);
	return 1;
}

CMD:orinar(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "PAULNMAC", "Piss_in", 4.0, 0, 0, 0, 0, 0);
	return 1;
}

CMD:fumar(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"NHumo12467ING", "M_smklean_loop", 4.0, 1, 0, 0, 0, 0);
	return 1;
}

CMD:sentarse(playerid,params[])
{
	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "SUNBATHE", "ParkSit_M_in", 4.000000, 0, 1, 1, 1, 0);
    return 1;
}

CMD:boxear(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "GYMNASIUM", "gym_shadowbox",  4.1,7,5,1,1,1);
    return 1;
}

CMD:cansado(playerid,params[])
{
	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"PED","WOMAN_runfatold",4.1,7,5,1,1,1);
    return 1;
}

CMD:hablar(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"PED","IDLE_chat",4.1,7,5,1,1,1);
	return 1;
}

CMD:taichi(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "PARK", "Tai_Chi_Loop",  4.1,7,5,1,1,1);
	return 1;
}

CMD:strip(playerid,params[])
{
	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"STRIP","strip_C",4.1,7,5,1,1,1);
	return 1;
}

CMD:bailar(playerid,params[])
{
	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
 	ApplyAnimation(playerid,"DANCING","dance_loop",4.1,7,5,1,1,1);
	return 1;
}

CMD:adolorido(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"PED","KO_shot_stom",4.1,0,1,1,1,1);
	return 1;
}

CMD:mujer(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
	ApplyAnimation(playerid,"PED","WOMAN_walkpro",4.0,1,1,1,1,500);
	return 1;
}

CMD:agredido(playerid,params[])
{
    PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid,"POLICE","crm_drgbst_01", 4.0, 0, 0, 0, 1, 0);
	return 1;
}

CMD:paja(playerid,params[])
{
 	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
    ApplyAnimation(playerid, "PAULNMAC", "wank_loop", 4.0, 1, 0, 0, 1, 0);
	return 1;
}

CMD:terminarpaja(playerid,params[])
{
 	PlayerPlaySound(playerid, 1150, 0.0, 0.0, 0.0);
 	ApplyAnimation(playerid, "PAULNMAC", "wank_out", 4.0, 0, 0, 0, 0, 0);
	return 1;
}

public OnPlayerEnterVehicle(playerid, vehicleid, ispassenger)
{
	return 1;
}

public OnPlayerExitVehicle(playerid, vehicleid)
{
	return 1;
}

public OnPlayerStateChange(playerid, newstate, oldstate)
{
	return 1;
}

public OnPlayerEnterCheckpoint(playerid)
{
	return 1;
}

public OnPlayerLeaveCheckpoint(playerid)
{
	return 1;
}

public OnPlayerEnterRaceCheckpoint(playerid)
{
	return 1;
}

public OnPlayerLeaveRaceCheckpoint(playerid)
{
	return 1;
}

public OnRconCommand(cmd[])
{
	return 1;
}

public OnPlayerRequestSpawn(playerid)
{
	return 1;
}

public OnObjectMoved(objectid)
{
	return 1;
}

public OnPlayerObjectMoved(playerid, objectid)
{
	return 1;
}

public OnPlayerPickUpPickup(playerid, pickupid)
{
	return 1;
}

public OnVehicleMod(playerid, vehicleid, componentid)
{
	return 1;
}

public OnVehiclePaintjob(playerid, vehicleid, paintjobid)
{
	return 1;
}

public OnVehicleRespray(playerid, vehicleid, color1, color2)
{
	return 1;
}

public OnPlayerSelectedMenuRow(playerid, row)
{
	return 1;
}

public OnPlayerExitedMenu(playerid)
{
	return 1;
}

public OnPlayerInteriorChange(playerid, newinteriorid, oldinteriorid)
{
	return 1;
}

public OnPlayerKeyStateChange(playerid, newkeys, oldkeys)
{
	return 1;
}

public OnRconLoginAttempt(ip[], password[], success)
{
	return 1;
}

public OnPlayerUpdate(playerid)
{
	return 1;
}

public OnPlayerStreamIn(playerid, forplayerid)
{
	return 1;
}

public OnPlayerStreamOut(playerid, forplayerid)
{
	return 1;
}

public OnVehicleStreamIn(vehicleid, forplayerid)
{
	return 1;
}

public OnVehicleStreamOut(vehicleid, forplayerid)
{
	return 1;
}

public OnDialogResponse(playerid, dialogid, response, listitem, inputtext[])
{
	switch(dialogid)
	{
		case DLOGIN:
		{
			if(response == 1)
			{
				if(udb_hash(inputtext) == Informacion[playerid][pKey])
				{
					INI_ParseFile(UserPath(playerid), "LoadUser_data", .bExtra = true, .extra = playerid);
					LogearJugador(playerid);
				}
				else
				{
					ShowPlayerDialog(playerid,DLOGIN,DIALOG_STYLE_PASSWORD ,"Login","Contraseña incorrecta, inténta nuevamente.","Entrar","Salir");
					return 1;
				}
			}
			else
			{
				SendClientMessage(playerid, -1, "Fuiste expulsado del servidor automáticamente.");
				Kick(playerid);
			}
			return 1;
		}
		case DREGISTER:
		{
			if (response == 1)
			{
				if(strlen(inputtext) > 24)
				{
					ShowPlayerDialog(playerid,DREGISTER,DIALOG_STYLE_PASSWORD ,"","La contraseña debe tener como máximo 24 carácteres.","Registrar","Salir");
					return 1;
				}
				if(isnull(inputtext))
				{
					ShowPlayerDialog(playerid,DREGISTER,DIALOG_STYLE_PASSWORD,"","Ingresa una contraseña","Registrar","Salir");
					return 1;
				}
				RegistrarJugador(playerid,inputtext);
				return 1;
			}
			else
			{
				SendClientMessage(playerid, -1, "Fuiste expulsado del servidor automáticamente.");
				Kick(playerid);
			}
			return 1;
		}
		case DCMDSADM:
		{
		    new cmda[256];
		    switch(listitem)
		    {
  		        case 0:
		        {
		            strcat(cmda, "{00FF00}/Spec (ID) {FFFFFF}- {00FFEF}Spectear a un usuario mediante la ID");
					strcat(cmda, "\n{00FF00}/SpecOff {FFFFFF}- {00FFEF}Desactivar el modo specteador");
					strcat(cmda, "\n{00FF00}/Kick - Kickear a un usuario mediante la IP + Razón");
		            ShowPlayerDialog(playerid, 1991, DIALOG_STYLE_MSGBOX, "{FFFF00}Comandos administrativos {FFFFFF}- {00FF00}Moderador", cmda, "Cerrar", "");
				}
				case 1:
				{
				    strcat(cmda, "{00FF00}/Spec (ID) {FFFFFF}- {00FFEF}Spectear a un usuario mediante la ID");
				    strcat(cmda, "\n{00FF00}/SpecOff (ID) {FFFFFF}- {00FFEF}Desactivar el modo specteador");
					strcat(cmda, "\n{00FF00}/SetHP (ID) (0 - 100) {FFFFFF}- {00FFEF}Setear la vida de un usuario mediante la ID");
					strcat(cmda, "\n{00FF00}/Kick {FFFFFF}- {00FFEF}Kickear a un usuario mediante la ID + Razón");
					strcat(cmda, "\n{00FF00}/Ban {FFFFFF}- {00FFEF}Banear a un usuario mediante la ID + Razón");
				    ShowPlayerDialog(playerid, 1992, DIALOG_STYLE_MSGBOX, "{FFFF00}Comandos administrativos {FFFFFF}- {FFFF00}Administrador", cmda, "Cerrar", "");
				}
				case 2:
				{
				    strcat(cmda, "{00FF00}/Spec (ID) {FFFFFF}- {00FFEF}Spectear a un usuario mediante la ID");
					strcat(cmda, "\n{00FF00}/SpecOff {FFFFFF}- {00FFEF}Desactivar el modo specteador");
					strcat(cmda, "\n{00FF00}/Kick (ID) (Razón) {FFFFFF}- {00FFEF}Kickear a un usuario mediante la ID + Razón");
					strcat(cmda, "\n{00FF00}/SetAr (ID) (0 - 100){FFFFFF}- {00FFEF}Setear la armadura de un usuario mendiante la ID");
					strcat(cmda, "\n{00FF00}/DarArma (ID) (WeaponID) (Munición) {FFFFFF}- {00FFEF}Darle un arma + munición a un usuario mediante la ID");
	    			strcat(cmda, "\n{00FF00}/Ira (ID) {FFFFFF}- {00FFEF}Ir a un respectivo usuario mediante la ID");
					strcat(cmda, "\n{00FF00}/Traer (ID) {FFFFFF}- {00FFEF}Traer a un usuario a tu posición mediante la ID");
				    ShowPlayerDialog(playerid, 1993, DIALOG_STYLE_MSGBOX, "{FFFF00}Comandos administrativos {FFFFFF}- {DF7401}Dueño", cmda, "Cerrar", "");
				}
			}
		}
	}
	return 1;
}

public OnPlayerClickPlayer(playerid, clickedplayerid, source)
{
	return 1;
}

forward SafeLogin(playerid);
public SafeLogin(playerid)
{
	new playername[MAX_PLAYER_NAME];
	GetPlayerName(playerid, playername, MAX_PLAYER_NAME);

	ClearChatbox(playerid);
	SetPlayerCameraPos(playerid,-2719.929931, -1209.007812, 195.455017);
	SetPlayerCameraLookAt(playerid,-2720.824218, -1213.840332, 196.375518);

	SendClientMessage(playerid, 0xDAD7F0FF, "Bienvenido al servidor, realiza lo que se te pida a continuación...");

	if(INI_Exist(playername))
	{
		INI_ParseFile(UserPath(playerid), "LoadUser_pass", .bExtra = true, .extra = playerid);
		ShowPlayerDialog(playerid,DLOGIN,DIALOG_STYLE_PASSWORD ," ","Ingresa tu contraseña.","Entrar","Salir");
		return 1;
	}
	else
	{
		ShowPlayerDialog(playerid,DREGISTER,DIALOG_STYLE_PASSWORD ," ","Esta cuenta no está registrada, introduce una contraseña.","Registrar","Salir");
		return 1;
	}
}

ClearChatbox(playerid){
	for(new i = 0; i < 40; i++){
		SendClientMessage(playerid, -1, "");
	}
	return 1;
}

forward RegistrarJugador(playerid, password[]);
public RegistrarJugador(playerid, password[]){
	if(IsPlayerConnected(playerid)){
		new INI:File = INI_Open(UserPath(playerid));
		INI_SetTag(File,"datos");
		INI_WriteInt(File,"Contra",udb_hash(password));
		INI_Close(File);
		GameTextForPlayer(playerid, "~G~~H~~H~CREANDO CUENTA..", 2500, 3);
	}
	return 1;
}

forward LogearJugador(playerid);
public LogearJugador(playerid){

	SetPlayerColor(playerid,-1);
	new string[150];

	format(string, sizeof(string), "¡Bienvenido {E0C3E3}%s{EAF0F2}!", NombreJugador(playerid));
	SendClientMessage(playerid, -1, string);

	TogglePlayerSpectating(playerid, 0);
	SetPlayerScore(playerid,1);
	GivePlayerMoney(playerid,1);
	return 1;
}

forward GuardarInfo(playerid);
public GuardarInfo(playerid)
{
	if(IsPlayerConnected(playerid))
	{
		new Nombre_PJ[MAX_PLAYER_NAME];
		GetPlayerName(playerid, Nombre_PJ, sizeof(Nombre_PJ));
		if(INI_Exist(Nombre_PJ))
		{
			new Archivo_PJ[13+MAX_PLAYER_NAME+1];

			format(Archivo_PJ ,sizeof Archivo_PJ, ARCHIVO_L, Nombre_PJ);
			new INI:File = INI_Open(Archivo_PJ);

			INI_SetTag(File,"datos");
			INI_WriteInt(File,"Admin",Informacion[playerid][pAdmin]);
			INI_WriteInt(File,"Score",Informacion[playerid][Score]);
			INI_WriteInt(File,"Bajas",Informacion[playerid][Bajas]);
			INI_WriteInt(File,"Muertes",Informacion[playerid][Muertes]);

			INI_Close(File);
		}
	}
 	return 1;
}

forward LoadUser_data(playerid,name[],value[]);
public LoadUser_data(playerid,name[],value[]){

	INI_Int("Admin",Informacion[playerid][pAdmin]);
	INI_Int("Score",Informacion[playerid][Score]);
	INI_Int("Bajas",Informacion[playerid][Bajas]);
	INI_Int("Muertes",Informacion[playerid][Muertes]);
	return 0;
}

forward LoadUser_pass(playerid,name[],value[]);
public LoadUser_pass(playerid,name[],value[]){
	INI_Int("Contra",Informacion[playerid][pKey]);
	return 1;
}

stock udb_hash(buf[]){ // Dracoblue
	new length=strlen(buf),s1 = 1,s2 = 0,n;
	for (n=0; n<length; n++){
		s1 = (s1 + buf[n]) % 65521,
		s2 = (s2 + s1)     % 65521;
	}
	return (s2 << 16) + s1;
}

stock INI_Exist(nickname[]){
	new tmp[128];
	format(tmp,sizeof(tmp),ARCHIVO_L, nickname);
	return fexist(tmp);
}

stock UserPath(playerid){
	new string[128],playername[MAX_PLAYER_NAME];
	GetPlayerName(playerid,playername,sizeof(playername));
	format(string,sizeof(string),ARCHIVO_L,playername);
	return string;
}

stock IsNumeric(string[]){
	for (new i = 0, j = strlen(string); i < j; i++){
		if (string[i] > '9' || string[i] < '0') return 0;
	}
	return 1;
}

stock NombreJugador(playerid){
	new NombrePJ[24];
	GetPlayerName(playerid,NombrePJ,24);
	new N[24];
	strmid(N,NombrePJ,0,strlen(NombrePJ),24);
	for(new i = 0; i < MAX_PLAYER_NAME; i++){
		if (N[i] == '_') N[i] = ' ';
	}
	return N;
}

stock EnviarMensajeAdmins(color, string[])
{
    foreach(Player, i)
    {
    	if(Informacion[i][pAdmin] >= 1)
    	{
        	SendClientMessage(i, color, string);
    	}
    }
}
