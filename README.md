# hello-world
Just for fun, nothing special

package net.apcat.simplesit.commands;

import net.apcat.simplesit.SimpleSit;
import net.apcat.simplesit.SimpleSitPlayer;
import net.apcat.simplesit.utils.Text;
import org.bukkit.command.Command;
import org.bukkit.command.CommandExecutor;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;

public class LayCommand
  implements CommandExecutor
{
  private SimpleSit simpleSit;
  
  public LayCommand(SimpleSit simpleSit)
  {
    this.simpleSit = simpleSit;
  }
  
  public boolean onCommand(CommandSender sender, Command command, String commandLabel, String[] args)
  {
    if (!(sender instanceof Player))
    {
      sender.sendMessage(Text.PLAYER_COMMAND.format(new Object[0]));
      return false;
    }
    if (!sender.hasPermission(this.simpleSit.getLayPermission()))
    {
      sender.sendMessage(Text.INVALID_PERMISSION.format(new Object[0]));
      return false;
    }
    SimpleSitPlayer player = new SimpleSitPlayer((Player)sender);
    if (player.isLaying()) {
      player.setLaying(false);
    } else if (player.getBukkitPlayer().isOnGround()) {
      player.setLaying(true);
    } else {
      player.getBukkitPlayer().sendMessage(this.simpleSit.getLayFailMessage());
    }
    return true;
  }
}
