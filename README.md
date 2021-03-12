using System;

// Token: 0x020000E4 RID: 228
public class LoadsMapMain
{
	// Token: 0x0600097C RID: 2428 RVA: 0x000A16D4 File Offset: 0x0009F8D4
	public static void LoadsMap1()
	{
		LoadsMapMain.posLeft = new int[2];
		LoadsMapMain.posMid = new int[2];
		LoadsMapMain.posRight = new int[2];
	}

	// Token: 0x0600097D RID: 2429 RVA: 0x000A1700 File Offset: 0x0009F900
	public static void LoadsMap2(int cfo, int cfp)
	{
		global::Char.myCharz().statusMe = -1;
		if (GameScr.canAutoPlay)
		{
			if (cfo < 15)
			{
				global::Char.myCharz().cx = 15;
			}
			else if (cfo > TileMap.pxw - 15)
			{
				global::Char.myCharz().cx = TileMap.pxw - 15;
			}
			else
			{
				global::Char.myCharz().cx = cfo;
			}
			global::Char.myCharz().cy = cfp;
			Service.gI().charMove();
			return;
		}
		global::Char.myCharz().cdir = ((global::Char.myCharz().cx - cfo < 0) ? 1 : -1);
		if (global::Math.abs(global::Char.myCharz().cx - cfo) > LoadsMapMain.toado)
		{
			LoadsMapMain.LoadsMap2(global::Char.myCharz().cx + LoadsMapMain.toado * global::Char.myCharz().cdir, LoadsMapMain.LoadsMap3(global::Char.myCharz().cx + LoadsMapMain.toado * global::Char.myCharz().cdir) - 24);
			LoadsMapMain.LoadsMap2(cfo, cfp);
			Service.gI().charMove();
		}
		if (global::Math.abs(global::Char.myCharz().cx - cfo) <= LoadsMapMain.toado)
		{
			if (cfo < 15)
			{
				global::Char.myCharz().cx = 15;
			}
			else if (cfo > TileMap.pxw - 15)
			{
				global::Char.myCharz().cx = TileMap.pxw - 15;
			}
			else
			{
				global::Char.myCharz().cx = cfo;
			}
			global::Char.myCharz().cy = cfp;
			Service.gI().charMove();
		}
	}

	// Token: 0x0600097E RID: 2430 RVA: 0x000A18C0 File Offset: 0x0009FAC0
	public static int LoadsMap3(int cfq)
	{
		int num = 50;
		int i = 0;
		while (i < 30)
		{
			i++;
			num += 24;
			if (TileMap.tileTypeAt(cfq, num, 2))
			{
				if (num % 24 != 0)
				{
					num -= num % 24;
				}
				return num;
			}
		}
		return num;
	}

	// Token: 0x0600097F RID: 2431 RVA: 0x000A1914 File Offset: 0x0009FB14
	public static void LoadsMap4(int keyAsciiPress)
	{
		if (keyAsciiPress == 0)
		{
			if (LoadsMapMain.posLeft[0] != 0 && LoadsMapMain.posLeft[1] != 0)
			{
				LoadsMapMain.LoadsMap2(LoadsMapMain.posLeft[0], LoadsMapMain.posLeft[1]);
			}
			else
			{
				LoadsMapMain.LoadsMap2(60, LoadsMapMain.LoadsMap3(60));
			}
		}
		else if (keyAsciiPress == 1)
		{
			if (LoadsMapMain.posRight[0] != 0)
			{
				if (LoadsMapMain.posRight[1] != 0)
				{
					LoadsMapMain.LoadsMap2(LoadsMapMain.posRight[0], LoadsMapMain.posRight[1]);
					goto IL_114;
				}
			}
			LoadsMapMain.LoadsMap2(TileMap.pxw - 60, LoadsMapMain.LoadsMap3(TileMap.pxw - 60));
		}
		else if (keyAsciiPress == 2)
		{
			if (LoadsMapMain.posMid[0] != 0)
			{
				if (LoadsMapMain.posMid[1] != 0)
				{
					LoadsMapMain.LoadsMap2(LoadsMapMain.posMid[0], LoadsMapMain.posMid[1]);
					goto IL_114;
				}
			}
			LoadsMapMain.LoadsMap2(TileMap.pxw / 2, LoadsMapMain.LoadsMap3(TileMap.pxw / 2));
		}
		IL_114:
		Service.gI().requestChangeMap();
	}

	// Token: 0x06000980 RID: 2432 RVA: 0x000A1A40 File Offset: 0x0009FC40
	public static void LoadsMap5()
	{
		LoadsMapMain.LoadsMap1();
		int num = TileMap.vGo.size();
		if (num != 2)
		{
			for (int i = 0; i < num; i++)
			{
				Waypoint waypoint = (Waypoint)TileMap.vGo.elementAt(i);
				if (waypoint.maxX < 60)
				{
					LoadsMapMain.posLeft[0] = (int)(waypoint.minX + 15);
					LoadsMapMain.posLeft[1] = (int)waypoint.maxY;
				}
				else if ((int)waypoint.maxX > TileMap.pxw - 60)
				{
					LoadsMapMain.posRight[0] = (int)(waypoint.maxX - 15);
					LoadsMapMain.posRight[1] = (int)waypoint.maxY;
				}
				else
				{
					LoadsMapMain.posMid[0] = (int)(waypoint.minX + 15);
					LoadsMapMain.posMid[1] = (int)waypoint.maxY;
				}
			}
			return;
		}
		Waypoint waypoint2 = (Waypoint)TileMap.vGo.elementAt(0);
		Waypoint waypoint3 = (Waypoint)TileMap.vGo.elementAt(1);
		if ((waypoint2.maxX < 60 && waypoint3.maxX < 60) || ((int)waypoint2.minX > TileMap.pxw - 60 && (int)waypoint3.minX > TileMap.pxw - 60))
		{
			LoadsMapMain.posLeft[0] = (int)(waypoint2.minX + 15);
			LoadsMapMain.posLeft[1] = (int)waypoint2.maxY;
			LoadsMapMain.posRight[0] = (int)(waypoint3.maxX - 15);
			LoadsMapMain.posRight[1] = (int)waypoint3.maxY;
			return;
		}
		if (waypoint2.maxX < waypoint3.maxX)
		{
			LoadsMapMain.posLeft[0] = (int)(waypoint2.minX + 15);
			LoadsMapMain.posLeft[1] = (int)waypoint2.maxY;
			LoadsMapMain.posRight[0] = (int)(waypoint3.maxX - 15);
			LoadsMapMain.posRight[1] = (int)waypoint3.maxY;
			return;
		}
		LoadsMapMain.posLeft[0] = (int)(waypoint3.minX + 15);
		LoadsMapMain.posLeft[1] = (int)waypoint3.maxY;
		LoadsMapMain.posRight[0] = (int)(waypoint2.maxX - 15);
		LoadsMapMain.posRight[1] = (int)waypoint2.maxY;
	}

	// Token: 0x06000983 RID: 2435 RVA: 0x000A1C7C File Offset: 0x0009FE7C
	public static void LoadsMap6(int cfs)
	{
		if (cfs == 0)
		{
			if (LoadsMapMain.posLeft[0] != 0 && LoadsMapMain.posLeft[1] != 0)
			{
				LoadsMapJKL.LoadsMapWin1(LoadsMapMain.posLeft[0], LoadsMapMain.posLeft[1]);
			}
			else
			{
				LoadsMapJKL.LoadsMapWin1(60, LoadsMapMain.LoadsMap3(60));
			}
		}
		else if (cfs == 1)
		{
			if (LoadsMapMain.posRight[0] != 0 && LoadsMapMain.posRight[1] != 0)
			{
				LoadsMapJKL.LoadsMapWin1(LoadsMapMain.posRight[0], LoadsMapMain.posRight[1]);
			}
			else
			{
				LoadsMapJKL.LoadsMapWin1(TileMap.pxw - 60, LoadsMapMain.LoadsMap3(TileMap.pxw - 60));
			}
		}
		else if (cfs == 2)
		{
			if (LoadsMapMain.posMid[0] != 0)
			{
				if (LoadsMapMain.posMid[1] != 0)
				{
					LoadsMapJKL.LoadsMapWin1(LoadsMapMain.posMid[0], LoadsMapMain.posMid[1]);
					goto IL_109;
				}
			}
			LoadsMapJKL.LoadsMapWin1(TileMap.pxw / 2, LoadsMapMain.LoadsMap3(TileMap.pxw / 2));
			IL_109:
			Service.gI().requestChangeMap();
		}
		Service.gI().getMapOffline();
	}

	public static int[] posLeft;

	public static int[] posMid;

	public static int[] posRight;

	public static int toado = 150;
}

--->
